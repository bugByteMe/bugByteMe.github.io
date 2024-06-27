## Generic unpack procedure

1. **Break the program where finishes the decrypt procedure.**

​	`If a large program has no relocation table, it must has been packed.`

​	Trace into the shell code.

​	**Shell code file offset:**

​		`Header size = [0x08]`

​		`Shellcode delta_cs = [0x16] `

​		`Shellcode ip = [0x14]`

​		`Shellcode file offset = (delta_cs + headsize)*0x10 + ip`

​		`first_seg = shellcode_addr - delta_shellcode`

​	Initial `es, ds` is the **PSP** address.

​	`es+10h` is the address of **first segment**.

​	**Original program length (most cases, no zip):**

​		 `Shellcode addr - (es+10h)*0x10`

​		or `(delta_cs)*0x10 + ip`



2. **Dump the decrypted code.**

​	**Bochs**

​		`writemem "filename" startAddr length`



3. **Analyze the relocation procedure in shell code.**

​		Example:

​		`add word ptr es:[di], bp`

​		`es` is delta cs

​		`di` is offset

​		`bp` is the address of the first segment



4. **Dump the relocation table.**

​		**Bochs**

​		`writemem "filename" startAddr length`



5. **Rebuilt the unpacked program**

​		**Rebuilt item Check list:**

​		a. Decrypted program

​		b. Relocation table

​		c.  ss `[0xE]`, delta_sp`[0x10]` (check this after jump to usr code)

​		d. ip `[0x14]`, delta_cs `[0x16]` (check this after jump to usr code)

​		e. relocation count `[0x6]`

​		f. page size `[0x4]` page rem `[0x2]` (header size + program size)

​		g. Header size `[0x8]` ceiling to 0x10