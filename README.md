# Step 0 : Installation
### Hello everyone.

- [x] I placed the downloaded weights into the correct directory 
`\checkpoints\ckpt-0`
- [x] Changed the run(main_parameters) to circumvent the mesh issue:
`ValueError: Number of devices 1 must equal the product of mesh_shape (1, 8)` -> changed (1, 8) -> (1, 1)
- [ ] Everything seemed fine, but then I encountered this error:
FileNotFoundError: [Errno 2] No such file or directory: '/dev/shm/tmpva2uzbug'

I will attempt to resolve this issue myself, but if anyone 
has already found a solution, please share it ❤️. 

Here is my PC configuration:
> Processor: 13th Gen Intel(R) Core(TM) i9-13900K (32 CPUs), ~3.0GHz
> Memory: 65536MB RAM
> Operating System: Windows 11 Pro 64-bit (10.0, Build 22631) (22621.ni_release.220506-1250)
> Card name: NVIDIA GeForce RTX 4080

Found the solution in other topics,

Everything works well if you apply this code:

checkpoint.py ->
```
@contextlib.contextmanager
def copy_to_shm(file: str):
    tmp_dir = tempfile.gettempdir()
    fd, tmp_path = tempfile.mkstemp(dir=tmp_dir)
    try:
        shutil.copyfile(file, tmp_path)
        yield tmp_path
    finally:
        os.close(fd)
        os.remove(tmp_path)


@contextlib.contextmanager
def copy_from_shm(file: str):
    tmp_dir = tempfile.gettempdir()
    fd, tmp_path = tempfile.mkstemp(dir=tmp_dir)
    try:
        yield tmp_path
        shutil.copyfile(tmp_path, file)
    finally:
        os.close(fd)
        os.remove(tmp_path)
```
# Step 1 : Testing

I launched the model and fixed all the bugs. <br>
However, I can't fully run it on my machine. 😞<br><br>
I've read forums where people mention the need for server-grade hardware.<br>
At minimum, 100GB of RAM and 5 video cards with more than 8GB of memory each.<br><br>
The best option would be to wait for a lighter version with fewer parameters. ⭐<br>
