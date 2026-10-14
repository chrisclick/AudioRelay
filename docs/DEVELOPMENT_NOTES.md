# Development Notes

This is a concise record of the main iterations behind Imaging Relay rather than a complete development log.

---

## Starting point

The original problem was making live Pro Tools audio easy to share during remote sessions.

Our normal production setup used Apollo Twin X interfaces and UA Console. I had previously built routing paths through UA Console and Pro Tools that could get session audio into Zoom, but the setup was cumbersome and required changing or maintaining additional routing around an otherwise working studio configuration.

Imaging Relay started as a way to simplify that process: leave the producer's normal Pro Tools and Apollo monitoring path alone, capture the DAW audio separately, and make it easy to share that audio in real time.

The microphone and supporting computer audio became additional sources as the prototype developed.

---

## First architecture test

Before building the full mixer, I tested the central assumption:

**Could Pro Tools be captured as an application while continuing to play through its normal monitoring path?**

If that worked, the producer would not need to reroute Pro Tools through the conferencing path just to share a session.

Once that was validated, the prototype expanded to include microphone and computer-audio sources.

---

## Major iterations

### From capture test to relay

After proving that Pro Tools could be captured independently of its normal monitoring output, the prototype expanded to include:

- microphone capture
- computer-audio capture
- per-source gain
- a stereo relay output
- setup guidance for using the relay during remote sessions

At that point, the project had moved beyond the original DAW-sharing experiment into a more complete live collaboration tool.

### Stale audio and delay

An early test produced several seconds of apparent delay. The issue came from stale queued audio rather than unavoidable conferencing latency.

Clearing buffered capture data corrected the problem in subsequent hands-on tests.

### OFF behavior and privacy

An early build kept source capture active even when a source was excluded from the mix.

Because the operating-system capture indicator remained active, the behavior was changed so OFF stops capture. Temporary preflight metering became a separate, explicit action.

### Multichannel microphone routing

Testing with production audio interfaces showed that microphone input could not be assumed to live on channel 1.

The input path was expanded to support:

- arbitrary input devices
- centered mono mapping
- stereo channel pairs
- non-default channel selection

### UI simplification

As routing features grew, the interface was split between:

- a focused main mixer
- compact mode
- deeper routing/settings controls
- an optional analyzer drawer

### Metering and analyzer refinement

Metering was adjusted over multiple iterations for readability and practical use with different kinds of material.

The analyzer added deeper signal information without changing the outgoing audio.

### Performance tuning

Analyzer and UI work initially used more CPU than was practical for a utility running beside Pro Tools.

Later passes reduced unnecessary work by:

- moving more signal processing to Accelerate/vDSP
- reducing repeated allocation/work in live paths
- simplifying visual publishing
- stopping detailed visual analysis when it was not visible

### Lifecycle recovery

Long-running tests exposed failures after display sleep/wake and other lifecycle changes.

Recovery logic improved through watchdogs and rebuild behavior, but pilot feedback indicates that wake recovery is not yet fully reliable on every machine/state.

---

## Features intentionally left out

The prototype does not attempt to be:

- a recorder/editor
- a waveform tool
- a mastering application
- a file conversion utility
- a general-purpose routing matrix

Those features were outside the original goal of making live session audio easier to share.

---

## AI-assisted development

The prototype was developed with substantial AI assistance.

I defined the requirements and expected behavior, used AI to help research unfamiliar macOS APIs and generate/revise implementation, then tested the resulting builds in the real workflow. Problems found during testing became the requirements for the next iteration.

---

## Current status

Imaging Relay remains a **working prototype / internal pilot**.

The core live-audio sharing workflow has been validated, while distribution, automated testing, and sleep/wake recovery would need additional work before treating it as a production release.
