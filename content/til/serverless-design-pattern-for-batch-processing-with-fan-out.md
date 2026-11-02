+++
title = "Serverless Design Pattern for Batch Processing with Fan Out"
date = 2023-03-03T06:00:00.000Z
tags = [ "AWS", "Serverless", "Design-Patterns" ]
+++

This is a simple serverless design pattern that has come in handy numerous times during my career. The problem: I need to process a large amount of data through a Lambda and am running into timeouts even at 15 minutes - or maybe I just need the process to hurry itself along.

## Serverless Batch Processing

The design pattern only requires a single Lambda, with a conditional variable in the input that chooses how the Lambda will behave. It will either operate as **A)** the fan-out orchestrator, or **B)** process a single item/batch.

{{< figure src="/uploads/serverless-batch-processing-smaller.jpg" alt="An external trigger starts a Lambda orchestrator, which splits data into chunks and invokes parallel workers using the same Lambda function." caption="One Lambda, two roles: start_fanout distributes the chunks; process_chunk handles each batch. The pseudocode follows below." >}}

The pseudocode from the photo is repeated below:

    action = input["action"] or "process_chunk"
    
    if action == "start_fanout":
      chunks = break_data_into_chunks(
            data, size=1000)
      for chunk in chunks:
        input = {
             action: "process_chunk",
             data_chunk: chunk
          }
        invoke_lambda("Lambda Name", input)
    elif action == "process_chunk":
        processing_func(data_chunk)
    else:
      # handle it 

In the example, `action` is our key conditional trigger.

### Variations

There's a few different ways you can vary the pattern to suit your needs. When input is first received to the `start_fanout` worker, it can either be passed as part of the input or just be used as the signal for the worker to go collect that information. 

In the same vein, sometimes it makes sense to pull all the data and chunk it before passing it to `process_chunk`, other times you can pass something like an ID so the processor knows where to pull the data it has to act on.

### Requirements

You'll need to have a way (and permissions!) to recursively invoke your function for this pattern to be an option. If on AWS, that might be **`lambda:InvokeFunction` **in the IAM role. A more widely usable pattern is available if your function sits behind an API - just make a request! Even the bare bones function runtimes provide the ability to make HTTP requests.
