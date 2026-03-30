I first opened the challenge in the browser and noticed that nothing useful appeared on the final page. The description mentioned that the flag pieces were stored in the redirect responses, so I knew I had to inspect the intermediate 302 responses instead of following them automatically.

The page source showed that the maze starts at /maze/1, so I requested that endpoint with curl:

curl -i http://chall-e614ed62.evt-246.glabs.ctf7.com/maze/1

That returned:

HTTP/1.1 302 Found
Location: /maze/2
X-Flag-Part: 1:ctf7{l

I then followed each Location manually:

/maze/2 -> X-Flag-Part: 2:ost_in
/maze/3 -> X-Flag-Part: 3:_redir
/maze/4 -> X-Flag-Part: 4:ects_9
/maze/5 -> X-Flag-Part: 5:5d44202}

At first I concatenated the full header values and got:

1:ctf7{l2:ost_in3:_redir4:ects_95:5d44202}

That was wrong because the number before the colon is just the fragment index, not part of the flag. Removing those indices and joining only the actual pieces gave:

**ctf7{lost_in_redirects_95d44202}**
