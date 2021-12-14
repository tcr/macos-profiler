# macos-profiler

A very basic profiler for Rust on macOS. It uses dtrace under the hood (invoked via shell, no less!) to log function calls at 1000Hz. This can give an approximate amount of information needed to debug CPU issues in your program.

The profiler has no deep `cargo` integration yet, meaning you'll have to specify the full path to your compiled binaries, but a hypothetical `cargo macos-profiler` could be able to discover `[[bin]]` paths more easily.

## Usage

```
cargo install macos-profiler
```

Then:

```
macos-profiler time-spent --command ./target/debug/my-binary
```

Lists functions in order of **how much time it spent in your stack hierarchy**.

```
macos-profiler frequency -c ./target/debug/other-binary
```

Lists functions in order of **how much time it spent the top of the stack, relative to all other top functions.**


### Example

```
$ cd ripgrep
$ sudo macos-profiler time-spent -c "./target/debug/rg "macos-profiler" /Users/tcr"
info: type ctrl+c to terminate your program and finish profiling.
dtrace: system integrity protection is on, some features will not be available

        ...
      0.76  20389    libsystem_kernel.dylib  _open
      0.77  20539                        rg  std::sys::unix::fs::File::open_c::hdfc195b95f7947a9
      0.78  21008                        rg  std::fs::OpenOptions::_open::he1336c8c2c1b3d49
      0.88  23674    libsystem_kernel.dylib  read
      1.04  27757                        rg  <rg::decoder::DecodeReader<R, B> as std::io::Read>::read
      1.54  41250                        rg  rg::search_stream::InputBuffer::fill::h178cd97cbf3a8408
      2.15  57728                        rg  <rg::search_stream::Searcher<'a, R, W>>::run
      2.18  58315                        rg  rg::worker::Worker::search::hff2f8efb10217782
      2.86  76557                        rg  rg::worker::Worker::run::h4a7a18cf12b9ee92
      2.94  78737                        rg  rg::run_parallel::_$u7b$$u7b$closure$u7d$$u7d$::_$u7b$$u7b$closure$u7d$$u7d$::hcb1f8a407164ade4
      4.04 108196                        rg  ignore::walk::Worker::run::he88d3c0c45c341d2
      4.04 108279                        rg  std::sys::unix::thread::Thread::new::thread_start::h65eccf0a2ed2beb8
      4.04 108280                        rg  std::sys_common::thread::start_thread::h2cb3c738dd903de2
      4.04 108280                        rg  <F as alloc::boxed::FnBox<A>>::call_box
      4.04 108280                        rg  std::panic::catch_unwind::h45c281ae72599974
      4.04 108280                        rg  std::thread::Builder::spawn::_$u7b$$u7b$closure$u7d$$u7d$::hfb393ad434cfed51
      4.04 108281                        rg  ignore::walk::WalkParallel::run::_$u7b$$u7b$closure$u7d$$u7d$::h56a4cd12003088da
      4.04 108281                        rg  std::sys_common::backtrace::__rust_begin_short_backtrace::hbe0d05bdae559442
      4.04 108281                        rg  std::panicking::try::do_call::h5e4bd48f15d06657
      4.04 108281                        rg  std::thread::Builder::spawn::_$u7b$$u7b$closure$u7d$$u7d$::_$u7b$$u7b$closure$u7d$$u7d$::h233c9e30f689939e
      4.04 108281                        rg  <std::panic::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
      4.04 108281                        rg  std::panicking::try::hdcb79b76b96f6366
      4.04 108282   libsystem_pthread.dylib  thread_start
      4.04 108296                        rg  _rust_maybe_catch_panic
      8.08 216562   libsystem_pthread.dylib  pthread_body
info: done.
```

## Contributing

Contributions welcome!

## License

MIT or Apache-2.0, at your option.
