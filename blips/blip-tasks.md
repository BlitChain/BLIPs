---
blip: tbd
title: [draft] Tasks
author: Sybil Singleton (@sybilsingleton), ChainTools (@qf3l3k)
status: Draft
type: Standard
category: Consensus
created: 2024-01-13
---

Simple Summary
--------------

Enables Blit accounts and scripts to autonomously run messages at scheduled times.

Abstract
--------

This BLIP introduces the ability for Blit accounts and scripts to schedule messages for future execution. This feature enables autonomous operations within the Blit ecosystem, allowing messages to be run at specified times or intervals without the need for external initiation.

Motivation
----------

The primary goal is to enhance the autonomy of Blit accounts and scripts. By enabling them to schedule messages for future execution, it allows for more complex and independent operations within the network.

Documentation
-------------

-   `MsgCreateTaskRequest`: Defines the task with details like activation time, frequency, expiration, and messages to be executed.
-   `MsgCreateTaskResponse`: Returns a unique task ID upon successful creation.
-   `Task`: Represents the scheduled task with both read-only and editable properties.
-   `PendingTask`: A queue or pool of tasks awaiting execution.
-   `TaskResult`: Records the outcome of each task execution, including successes and errors.
-   `BlitParams`: Defines network parameters related to task scheduling and execution.

Specification
-------------

-   Tasks are created using `MsgCreateTaskRequest` and identified with a unique task ID.
-   A task's execution is based on its activation and expiration times, frequency, and max runs.
-   The network manages a pool of pending tasks, executing them based on available resources and task priority.
-   Results of each execution are recorded in `TaskResult`.
-   Network parameters in `BlitParams` govern the overall operation of task scheduling and execution.


## Specification

```
MsgCreateTaskRequest {
  creator: address (signer)
  activate_on: block height int | timestamp str
  next: block height int | timestamp str | relative block "+x"
  expire_on: block height int | timestamp str | relative block "+x"
  max_runs: int
  gaslimit: int
  gasprice: coin
  messages: [msg, ...]
  enabled: bool
  stop_on_failure: bool
}

MsgCreateTaskResponse {
  task_id: int
}

Task {
  // readonly
  index: (creator, task_id)
  creator: address (signer)
  task_id: int
  total_run_count: int
      
  next_task_result_index: '' // look up and update, slash seperated

  // editable
  activate_on: block height int | timestamp str
  expire_on: block height int | timestamp str

  // either
  interval: block height int | duration str
  frequency: block height int | duration str
  
  max_runs: int 
  disable_on_error: bool
  enabled: bool 

  gas_limit: int
  gas_price: coin
  messages: [msg, ...]
}

PendingTask {
    - "queue", ("block" or "time"), when, task.gas_price, task.index
    - "pool", task.gas_price,  ("block" or "time"),  when,  task.index
    when
}

TaskResult {
  index:
    - task.index, "success" | "error", ("block" or "time"),  when

  id: int
  status: "queue", "pool", "success", "error"
  task_index
  when: int | string

  scheduled_on: block height int | timestamp str
  executed_on: block height int | timestamp str
  responses: []
  error: ''
  
}

BlitParams {
  ...
  min_gas_price: Coin
  run_pool_gas_limit: int // cumulative gas reserved for run tasks
  remaining_pool_gas_limit: int // remaining tasks in the pool that haven't been run

  // "when" must be between
  max_blocks_in_the_future
  max_time_in_the_future

  // remove the lowest gasprices tasks and set a timeout error
  max_pool_tasks: int
}
```

#### Pseudocode
```
for pending_task in PendingTasks
  task = pending_task.task

  if the pending_task > than the current block or timestamp:
    break
    
  change pending_task.index to pool type
  if task.frequency:  
    create new PendingTask if not past task.expires_on
    set on Task

for pool_task in PoolTasks:
  task = pool_task.task
  if task.expires_on < than the current block or timestamp:
    delete pending_task
  if not task.enabled:
    delete pending_task
    
  if run_pool_gas + task.gaslimit < run_pool_gas_limit
    run_pool_gas += task.gaslimit
    run task.messages
    create TaskResult
    delete pending_task
    if task.interval:
      create new PendingTask if not past task.expires_on
    run_pool_gas += pending_task.gaslimit
  else:
    if pending_cumulitive_gas + task.gaslimit < remaining_pool_gas_limit:
      pending_cumulitive_gas += task.gaslimit
    else:
      delete the pending_task
      create TaskResult with error
      if task.disable_on_error:
        task.enabled = false
      else
        if task.interval:
          if not past task.expires_on
            create new PendingTask 
```

## Drawbacks

Care must be taken in selecting params so that it is not possible to timeout the block while executing the pool tasks or having too many pending tasks.

## Rationale

@qf3l3k said
> Thinking more detailed - scheduling mechanism in Dyson was quite interesting and I think might be worth to add this to Blit and develop further into something similar to restake - like scheduler with web interface, maybe even with some pre-defined functions like "pull wallet balance(s)", "gather stake amounts from validators and store on ipfs in form of text file" - all that could be available via website similar to restake, where users would authorize execution of tasks. Reason I was thinking of pre-defined tasks is to avoid malicious script being deployed for execution on-chain, so then new functionality would be published as open source and has to go through governance to be added then as another building block.

@sybilsingleton said
> consumer use cases:
> 1. I want something to absolutely happen on a specific block height, i will pay for the certainty
> 2. I want something to happen on or after a block but I'm flexible when if it saves costs
> 3. I want something to happen on or after a timestamp but I'm also flexible when
> 4. I want it to reoccur again on an interval
> 5. I want to stop at a time, block, or max number of runs```

@qf3l3k says:
> use-cases look good. to point 2 I would add one block before certain block, so we have all cases covered.
additional thought... for scheduled tasks there must be some sort of subscription which has fixed time and has to be renewed, otherwise there will be lot of abandoned tasks running over time. with subscription once time reached and not renewed task is disabled/removed from execution schedule.


## Prior Art

- Dyson MsgCreateScheduledRun
  - https://dys.dysonprotocol.com/commands?command=dyson/sendMsgCreateScheduledRun
- https://github.com/CronCats

## Unresolved Questions

- Is it possible to gaurentee execution at a specific block?
  - Maybe not, because even normal TXs are not gaurenteed.
- Is is possible to cancel a Pending Task?
- Is it possible to adjust the gas price of an existing Pending Task?
  - With rebates if lowering the price?


## Backwards Compatibility

None

## Security Considerations

It's imoportant to make the gas limits low enough that maxmuim script usage never exceeds the block timeout.

## Future Possibilities

Possibly have subscriptions to event based triggers and not only time triggers.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
