# VNT WASM

VNT wasm is a virtual machine for VNTChain, to run the smart contracts written as wasm language code.

It is developed based on a wasm interperter `wagon`, which is a [WebAssembly](http://webassembly.org)-based interpreter in [Go](https://golang.org), for [Go](https://golang.org).

WebAssembly (short as WASM) is a representation language for stack-based virtual machine.

Many languages could be compiled into wasm code, like C/C++, python, Golang, etc.

**NOTE:** `wagon` requires `Go >= 1.9.x`.

# Memory

VNT wasm implements a new memory management mechanism. With this mechanism, we could support following data types:

- int, int32, int64
- uint, uint32, uint64, uint256
- string
- address
- mapping
- array
- struct

## Useage Sample

```go
package main

import (
	"github.com/vntchain/vnt-wasm/wasm"
	"bytes"
	"fmt"
	"github.com/vntchain/vnt-wasm/validate"
	"github.com/vntchain/vnt-wasm/exec"
	"github.com/vntchain/vnt-wasm/vnt"
)

// prepare a function to read an exported module
// this module could provide some host functions
// which is used by your contract.
// Of course, you could provide an empty module
func ResolveImports(name string) (*wasm.Module, error) {
	return &wasm.Module{}, nil
}

// initialize your wasm code, perhaps read it from a file
var code []byte

// prepare a function to do the memory initialization
func initMem(m *vnt.WavmMemory, module *wasm.Module) error {
	return nil
}

func main() {
	buf := bytes.NewReader(code)
	
	// read the module based on the code
	m, err := wasm.ReadModule(buf, ResolveImports)
	if err != nil {
		fmt.Errorf("could not read module", "err", err)
		return
	}

	// validate the module
	err = validate.VerifyModule(m)
	if err != nil {
		fmt.Errorf("could not verify module", "err", err)
		return
	}

	// initialize the vm
	vm, err := exec.NewInterpreter(m, nil, initMem)
	
	vm.ExecCode(1, 1, 2);
}
```


## VNT Smart Contract Tutorial

See the document [smart contract documents](https://github.com/vntchain/vnt-documentation/tree/master/smart-contract)
