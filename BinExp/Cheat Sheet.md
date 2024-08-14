

## juicycheat sheet
```sh
# find addr of str in libc
strings -a -t x /lib/libc-2.11.2.so | grep '/bin/'
```
## GDB
```sh
#
set disassembly-flavor intel

layout asm

info functions
break *main
info registers
st # step one instr
ni # step one execution - skip inner function calls
info proc mappings # permissions
break *addr
set $eax=0

define hook-stop
info registers
x/24wx $esp
x/10i $eip
end

# get addr of system()
p system
find start,end, "text" # find addr of txt

```


```sh
objdump -d file
objdump -x file
strace ./file # trace systemcalls
ltrace ./file # trace lib calls

```

## R2
```sh
aaa # analyze fncs
afl # prnt fncs
s sys.main # got to main loc
pdf # disas

###
VV # visual mode 
p # change visual representation
? help
: # run commands
###
r2 -d file # dbg
db addr # breakpoint
dc # run program
s # step instr
S # step call execution (skip fnc calls)

```


## juice
```sh
man 3 strcmp
man syscalls


```