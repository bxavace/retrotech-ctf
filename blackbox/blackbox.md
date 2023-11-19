# BlackBoxServer
Category: HARD, `2599` points

The flag is inside the simple Flask server code. 
Wait. 
What? 
Where's the server code?!

# Solution

1. There are several tutorials in the internet in de-obfuscating PyArmor library. [[1]](https://medium.com/@liad_levy/reverse-pyarmor-obfuscated-python-script-using-memory-dump-technique-9823b856be7a), [[2]](https://reverseengineering.stackexchange.com/questions/25075/reversing-pyarmor-discussion), [[3]](https://github.com/Svenskithesource/PyArmor-Unpacker), [[4]](https://www.youtube.com/watch?v=5iIbry7uOxY&pp=ygUYcHlhcm1vciByZXZlcnNlIGVuZ2luZWVy).

2. One example is to use memory dump code from [[1]](https://medium.com/@liad_levy/reverse-pyarmor-obfuscated-python-script-using-memory-dump-technique-9823b856be7a):

```python
# memdump.py
# https://gist.githubusercontent.com/Dbof/b9244cfc607cf2d33438826bee6f5056/raw/aa4b75ddb55a58e2007bf12e17daadb0ebebecba/memdump.py
#! /usr/bin/env python3
import sys
import re

if __name__ == "__main__":

    if len(sys.argv) != 2:
        print('Usage:', sys.argv[0], '<process PID>', file=sys.stderr)
        exit(1)

    pid = sys.argv[1]

    # maps contains the mapping of memory of a specific project
    map_file = f"/proc/{pid}/maps"
    mem_file = f"/proc/{pid}/mem"

    # output file
    out_file = f'{pid}.dump'

    # iterate over regions
    with open(map_file, 'r') as map_f, open(mem_file, 'rb', 0) as mem_f, open(out_file, 'wb') as out_f:
        for line in map_f.readlines():  # for each mapped region
            m = re.match(r'([0-9A-Fa-f]+)-([0-9A-Fa-f]+) ([-r])', line)
            if m.group(3) == 'r':  # readable region
                start = int(m.group(1), 16)
                end = int(m.group(2), 16)
                mem_f.seek(start)  # seek to region start
                print(hex(start), '-', hex(end))
                try:
                    chunk = mem_f.read(end - start)  # read region contents
                    out_f.write(chunk)  # dump contents to standard output
                except OSError:
                    print(hex(start), '-', hex(end), '[error,skipped]', file=sys.stderr)
                    continue
    print(f'Memory dump saved to {out_file}')
```

3. Run the obfuscated code (`whatami.py`) and go find the process ID. Assuming that the computer is UNIX-based (Mac), the command are as follows:

```
$ ps -efww | grep python whatami.py
```

or 

```
$ ps -efww | grep python | grep LISTEN
```

This command will list all of the processes running on your system that are using the Python interpreter and the server.py file. The PID of the Flask server will be listed in the first column of the output.

4. Using that PID to de-obfuscate the file, use the memory dump script (assuming the file is `memorydump.py`) to find clues:
```
$ python memorydump.py <PID>
```

5. Convert that to readable strings:

strings \<PID\>.dump > process.dump

6. Extract the information as needed.

**Flag:** `RETROTECH{d0_y0u_kn0W_r3v3rs3_eNg1n33r}`
