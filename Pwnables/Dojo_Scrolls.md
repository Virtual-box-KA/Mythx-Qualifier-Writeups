I began by connecting to the remote service and inspecting the available functionality. The program presented itself as a simple note-taking application:

1) Create note
2) Edit note
3) Show note
4) Exit

Only three notes could exist, with indices from 0 to 2. Since there were only a few objects and the challenge description mentioned that “when a scroll outgrows its space, the ink bleeds onto the next”, it strongly suggested a heap overflow into an adjacent note.

My first step was to understand how the notes were allocated. I created three notes of the same size so that their heap chunks would most likely be placed consecutively in memory:

1
0
16
AAAAAAAAAAAAAAAA

1
1
16
BBBBBBBBBBBBBBBB

1
2
16
CCCCCCCCCCCCCCCC

At this point the heap layout would look roughly like:

[note0 data chunk][note1 data chunk][note2 data chunk]

Next, I tested the edit functionality. The important detail was that when editing a note, the program did not preserve the original size. Instead, it asked for a completely new size:

2
0
64
...

This is the key bug. Note0 was originally allocated with only 16 bytes, but the edit operation allowed me to write 64 bytes into that same buffer. That means the program was writing past the end of note0 and into the memory belonging to note1.

The vulnerable logic was likely similar to:

notes[idx].data = malloc(size);
notes[idx].size = size;

during note creation, but during edit it probably did something like:

read(0, notes[idx].data, new_size);

without checking whether new_size <= notes[idx].size.

As a result, the extra bytes overflowed into the next heap chunk. Since note1 was allocated immediately after note0, its internal metadata could be corrupted.

The note structure was most likely something close to:

struct note {
    size_t size;
    char *ptr;
};

So by overflowing from note0 into note1, I could overwrite note1’s fields. The most useful field to corrupt was the pointer.

I edited note0 with a long payload such as:

AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

and then checked what happened when displaying note1:

3
1

Initially this either printed garbage or crashed, which confirmed that note1’s metadata had been corrupted successfully. From there, I adjusted the overflow length until I found the exact offset needed to overwrite only note1’s pointer field.

The idea was to replace note1’s normal heap pointer with the address of the hidden flag buffer stored somewhere else in the binary. In many challenges like this, there is a global variable similar to:

char flag[64] = "ctf7{...}";

or the flag is loaded into memory during startup.

Once the pointer for note1 had been replaced with the address of that flag buffer, the “show” functionality became an arbitrary read primitive. Instead of printing note1’s original contents, it printed whatever was located at the forged address.

After overwriting the pointer, I ran:

3
1

and the program printed the contents of the flag buffer directly:

ctf7{...}

So the complete exploitation chain was:

Create several notes so their chunks are adjacent on the heap.
Use the vulnerable edit function on note0 with a larger size than originally allocated.
Overflow into note1 and corrupt its metadata.
Overwrite note1’s pointer so it points to the flag in memory.
Use “show note1” to print the flag.

This is a very standard beginner heap exploitation pattern: a heap overflow gives control over a neighboring object, and then a “show” function is abused to leak arbitrary memory.
