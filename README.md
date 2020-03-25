# The problem
Older Windows Versions like Windows 2000 have debugging symbols in form of a 
.dbg file and a .pdb file, where the .pdb file ususally is a bit older and 
the .dbg file is up-to-date.
If you want to analyze such Files with IDA Pro, it seems like the symbol 
resolution doesn't work properly. The function start and ends are wrong and
the name resolution doesn't work either.
I suspect that IDA just parses the PDB and doesn't take the OMAP information
from the .dbg file isn't used. Anyway, the dbghelp.dll library does it right
and resolves the correct addresses to the function names.

# The solution
So in order to get at least correct symbol name information, I found a tool 
that converts .pdb files to .map files (via dbghelp) and modified it so spit 
out a .idc IDA script file, that contains code that does the name resolution
via MakeName IDA function, as IDA doesn't support .map files.

# Usage
The pdb2map tool got a new comamndline switch /i for creation of .idc files.
1. Place .pdb and .dbg file together into the directory of the file you 
   want to dump symbols vor
2. Run pdb2map /i yourfile.ext
   i.e.:  pdb2map /i winsrv.dll
3. When loading the file to be analyzed in IDA, refuse to take the .pdb file
   Then load the .odc generated in step 2 via File/Script file...
   After execution, function should be named correctly

# Contact
If you have questions or comments, feel free to contact me 
leecher@dose.0wnz.at
