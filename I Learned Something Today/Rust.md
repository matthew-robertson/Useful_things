1. People who use Rust are called "Rustaceans". This won't help you write Rust, but it *is* upsetting.
1. The last expression in a block is implicitly returned. If the last such line ends in a semicolon, it'll return Nil.
1. You still have access to the `return` keyword though, so probably just use that.
1. Every expression is able to be evaluated. That includes blocks.
1. "a word of warning by the way, certain data structures with 'unclear ownership semantics' are hellishly hard to write in Rust without unsafe code. An example of this would be a doubly linked list" (Alec, when talking about learning Rust)
1. Some [decent](https://doc.rust-lang.org/book/) [books](https://doc.rust-lang.org/rust-by-example/) exist on the subject of learning. One [terrifying](https://rust-unofficial.github.io/too-many-lists/) one too.