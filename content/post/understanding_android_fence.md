---
date: 2019-09-14
title: Understanding Android Fence Mechanism
weight: 10
---

## Introduction

There are a lot of articles discussing about Fence mechanism in Android. But I
could not find a simple explanation.

## What Problem Fence try to resolve?

### Problem 1: Producer pass buffer to consumer before consumer could use the buffer

In Android, there is an object called `GraphicBuffer`, which is passed in
between different kind of `producuers` and `consumers`. When a `producer` pass
a `GraphicBuffer` to a `consumer`, It is possible that the `producer` is still
doing some job on that buffer in background, because the producer may have told
GPU to render the buffer. For performance consideration, CPU could not wait for
GPU to finish the job before hand over the buffer to `consumer`. 

Fence was invented to allow to pass the ownership before the buffer is ready to
be consumed.

### Problem 2: The Producer would dequeue the buffer before it could overwrite the buffer

The same problem exists when the consumer finishes using the buffer by
releasing a buffer to buffer queue, but there are some background jobs ongoing
which is using the buffer. The producer could dequeue the buffer but have to
wait for the fence to be signaled before it could actually overwritten the
buffer.

See following description in *frameworks/native/include/gui/ConsumerBase.h*

```
        // mFence is a fence which will signal when the buffer associated with
        // this buffer slot is no longer being used by the consumer and can be
        // overwritten. The buffer can be dequeued before the fence signals;
        // the producer is responsible for delaying writes until it signals.
        sp<Fence> mFence;
```

## How it works

When a consumer `acquireBuffer()`, it will also copy/get the fence that in the
future it could wait on.


```

status_t ConsumerBase::acquireBufferLocked(BufferItem *item,
        nsecs_t presentWhen, uint64_t maxFrameNumber) {
...

    status_t err = mConsumer->acquireBuffer(item, presentWhen, maxFrameNumber);
...

    mSlots[item->mSlot].mFence = item->mFence;
...
}

```

Above code acquire a buffer from buffer queue and save the fence 

