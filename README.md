## Wayland-compatible emacs for Alpine Linux
### Patch:
Unfortunately glibc malloc_info and musl incompatible, patch removes function from ./src/alloc.c

## Build:
`
abuild checksum; 
abuild -r
` 
