# Start by printing "Hello, World!"
> A good start is half of the success.

## Four ways
The following is a simple example:
```zig
const std = @import("std");

pub fn main() !void {
	std.debug.print("Hello, World!", .{});
}
```
It is easy to understand if you have a basic of c language.

**However**, there is more than one way to print.
Such as using 'log':
```zig
const std = @import("std");

const log = std.log;

pub fn main() !void {
	log.info("Hello, World!", .{});
	// or err() warn()
}
```
*We don't care about the sementic, because our aim is to print "Hello, World!"*

using 'fmt':
```zig
const std = @import("std");

const io = std.io;
const fmt = std.fmt;

pub fn main() !void {
	const stdout = io.getStdOut().wirter();
	fmt.format(stdout, "Hello, World", .{});
}
```

and using 'io':
```zig
const std = @import("std");

const io = std.io;

pub fn main() !void {
	const stdout = io.getStdOut().wirter();
	const stderr = io.getStdErr().wirter();

	try stdout.print("Hello, World!", .{});
	try stderr.print("Hello, World!", .{});

	try stdout.wirterAll("Hello, world!");
}
```
**Please pay attention: the `try` is necessary, because the functon
will return the err, you should receive it.**

## Camparing
Although there are many ways, the first three are based on forth methods.
This can be seen in the source code:
```zig
// defaultLog() in log
const stderr = std.io.getStdErr().writer();
std.debug.getStderrMutex().lock();
defer std.debug.getStderrMutex().unlock();
nosuspend stderr.print(level_txt ++ prefix2 ++ format ++ "\n", args) catch return;

// print() in debug
stderr_mutex.lock();
defer stderr_mutex.unlock();
const stderr = io.getStdErr().writer();
nosuspend stderr.print(fmt, args) catch return;

// format() in fmt
if (start_index != end_index) {
	// the writer is "stderr" or "stdout"
	try writer.writeAll(fmt[start_index..end_index]);
}
```
From the source code, we can found that whether it is "log" or "debug", 
they both output to stderr.

The function of printing is common,
so there is no sence to campare with other programming languages.

*You don't know the "lock" or "nosuspend", which is about
"Asyns and Await", without them, we still can print "Hello, World!" well.*

## In summary
The four ways has a similar function, but they have different sementic.
In my opinion, we should follow the sementic, instead of it, the reader will
have difficulty understanding, which break the purpose of ziglang's creation
that communicate intent precisely.

So the best way to print "Hello, Wrold!" should be the forth way. Why?
Because it simply prints, it fits our goals that only want to print "Hello, Wrold!" prefectly. 

> As the saying goes:
> All roads lead to Rome, but only one way is most convenient.
