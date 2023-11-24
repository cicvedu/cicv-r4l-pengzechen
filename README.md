# cicv-r4l-pengzechen
cicv-r4l-pengzechen created by GitHub Classroom  
I finished homeworkd 1, 2, 3

## Homework 1(compile linux kernel)
The compiled kernel as pic vmlinux.png show.
(I remember that markdown can insert some pics, but I forget... )
## Homework 2(change some config)
Q1: We write some code in Kconfig like this :
~~~
config HELLO_WORLD
	tristate "Hello World"
	help
	  This is a simple hello world rust-module for rust learning.
~~~
when the compiler compile the kernel, it check .config file. There are two cases:  
 compiled as a dynamic moudel.
 compiled as a part of kernel (be linked)
then the compiler search symbols in spesific file like "helloworld.c"
finally compile it to binary file like helloworld.ko or helloworld.o  
Q2: I think the Makefile make difference
~~~
KDIR ?= ../linux

default:
	$(MAKE) -C $(KDIR) M=$$PWD

~~~
when we excute make, it will goto linux kernel to find source or interface when needed.  
If you move "src_e1000" folder, the compiler will not find source.

1. Reconfig result is shown as re-config.png (remove original Internet Interface)
2. ping result is shown as ping.png (use r4l_e1000 IF)
## Homework 3(write a simple helloworld moudle)
1. Write a simple rust_helloworld.rs source code as follows:
~~~
  use kernel::prelude::*;
      
module! {
  type: RustHelloWorld,
  name: "rust_helloworld",
  author: "whocare",
  description: "hello world module in rust",
  license: "GPL",
}
      
struct RustHelloWorld {}
      
impl kernel::Module for RustHelloWorld {
  fn init(_name: &'static CStr, _module: &'static ThisModule) -> Result<Self> {
      pr_info!("Hello World from Rust module\n");
      Ok(RustHelloWorld {})
  }
}
~~~
2. reconfig the menuconfig use "make LLVM=1 menuconfig", so that the moudle will be
   added as compiled, then recompile the kernel. I wrote Kconifg and Makefile 
~~~
  config HELLO_WORLD
  	tristate "Hello World"
  	help
  	  This is a simple hello world rust-module for rust learning.
  obj-$(CONFIG_HELLO_WORLD)		        += rust_helloworld.o
~~~
3. Do some test. I reboot the VM excute "insmod rust_helloworld.ko"
   output as pic print-helloworld.png shown.
