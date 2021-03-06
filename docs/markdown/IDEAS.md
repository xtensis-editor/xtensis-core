# Xt

## Goals

- Minimal
- Extendable
- Speedy

## Key features

- Package/plugin support.
  - JSON-API interface to `Xt/core`

- Encourage customisation.
  Xt should encourage customisation of the core, plugins and
  other components.

- Provide a set of defaults.
  Right now, I'm going with GNU Readline style keybindings.

- Written in Rust.

- Extendable and fast.

## Core functions

- Decentralised package manager

  This would be inspired in part from QUELPA & MELPA.

  QUELPA is a package manager for GNU Emacs based upon the MELPA package
  scheme.

  It is my aim to provide both a decentralised package manager, where
  packages can be retrieved from 'nodes' in a P2P fashion, and also
  where the source code (from `master`, or perhaps a commit hash), is
  stored on a Git repository.

- Major & Minor modes
    Emacs has support for 'major modes', which essentially define the
    syntax colouring schema, hooks to be run, programming indentation,
    etc.

    Emacs *also* has support for 'minor modes' which essentially
    modify behaviour in each major-mode (buffer-local basis) - this
    could be 'hard wrapping', syntax checking or spell checking.

    It is my intention to base Xt upon this model of major and minor
    modes, and use that to the highest advantage available.

### Optional client/server architecture

We should aim for a high startup speed for both the core, and
frontend, but the client/server architecture would be good to have as
well, a la `emacsclient` and `emacs` pair.

### Backend / Frontend infrastructure

This is inspired from the xi-editor project, which I found to
be an excellent way to have enhanced UI flexibility.

The backend(core) and frontend would communicate via JSON-RPC, also
inspired from xi-editor.

The frontend should request the bare minimum of buffer contents to
save transfers. For example, if the frontend is instructed to move
down five lines, it will retrieve 5 lines worth of data in an JSON
array, similar to pagination.

(I got this idea from @spudowiar who mentioned Xi's editor method of
transferring data frontend <--> backend, and how it was
inefficient. @spudowiar has the right idea here.)

### Frontend

#### Frontend boot-up

The frontend will bootstrap itself on launch (this should be quick!!)
, and if there's no server available, launch, wait for a `READY`
signal, and then start the frontend.

Xt-core should have API's to deal with everyday tasks.

## Addons

## Permission model

Like the Android's permissions model, I have been toying with the idea
of permissions for Xt.

The permissions model would be along the lines of this:

(This is *not* a definitive list yet)

- `Xt.permissions.buffers.read_buffer`
  Arguments: `buffer_id`

   This permission grants access to read a buffer's contents.
   At the time of writing, this does *not* grant read access on a
   per-line basis, but that would be something to look into in future.

- `Xt.permissions.buffers.line_range_read`
  Arguments: `line_begin`, `line_end`

  This would allow 'ranges' of lines to be read by the text editor.

- `Xt.permissions.buffers.edit_buffer`
  Arguments: `buffer_id`

  This permission grants access to edit a buffer's contents.

  This gives all-area write access to the buffer, and does not
  restrict on a per-line basis.

- `Xt.permissions.modes.minor_hook.add`
  Arguments: `buffer_id`, `hook_id`

  This permission grants access for a plugin to hook into a major
  mode.

  Ideally, **no** third party plugin should be allowed to hook into a
  buffer/mode by default. But that is just a convention, and _can_ be
  overridden.

To the best of my knowledge, *no* other editor supports a permissions
model, and thus, `Xt` would be the first editor of its kind to
*ever* implement permissions.

## External process communication

Xt will support stdio communication between xt-core and external
processes. This feature will be known as "ports".

## External libraries

Xt will support (with potential instability) external libraries using
the FFI module.

This can be unstable, and should be implemented in such a way to
mitigate Xt core crashes.
