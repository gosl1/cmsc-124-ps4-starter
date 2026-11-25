# The Search and the Stack

Replace this paragraph after your checks pass. Paste the output of `./trace`
for one case. Then answer three questions about it.

$ ./trace P08
== trace P08 ==
Call: ancestor(tom,X)
  Call: parent(tom,X)
  Exit: parent(tom,liz)
Exit: ancestor(tom,liz)
answer 1: X=liz
Redo: ancestor(tom,liz)
  Redo: parent(tom,liz)
  Exit: parent(tom,bob)
Exit: ancestor(tom,bob)
answer 2: X=bob
Redo: ancestor(tom,bob)
  Redo: parent(tom,bob)
  Fail: parent(tom,X)
  Call: parent(tom,_)
  Exit: parent(tom,liz)
  Call: ancestor(liz,X)
    Call: parent(liz,X)
    Fail: parent(liz,X)
    Call: parent(liz,_)
    Fail: parent(liz,_)
  Fail: ancestor(liz,X)
  Redo: parent(tom,liz)
  Exit: parent(tom,bob)
  Call: ancestor(bob,X)
    Call: parent(bob,X)
    Exit: parent(bob,ann)
  Exit: ancestor(bob,ann)
Exit: ancestor(tom,ann)
answer 3: X=ann
Redo: ancestor(tom,ann)
  Redo: ancestor(bob,ann)
    Redo: parent(bob,ann)
    Exit: parent(bob,pat)
  Exit: ancestor(bob,pat)
Exit: ancestor(tom,pat)
answer 4: X=pat
Redo: ancestor(tom,pat)
  Redo: ancestor(bob,pat)
    Redo: parent(bob,pat)
    Fail: parent(bob,X)
    Call: parent(bob,_)
    Exit: parent(bob,ann)
    Call: ancestor(ann,X)
      Call: parent(ann,X)
      Exit: parent(ann,sue)
    Exit: ancestor(ann,sue)
  Exit: ancestor(bob,sue)
Exit: ancestor(tom,sue)
answer 5: X=sue
Redo: ancestor(tom,sue)
  Redo: ancestor(bob,sue)
    Redo: ancestor(ann,sue)
      Redo: parent(ann,sue)
      Fail: parent(ann,X)
      Call: parent(ann,_)
      Exit: parent(ann,sue)
      Call: ancestor(sue,X)
        Call: parent(sue,X)
        Fail: parent(sue,X)
        Call: parent(sue,_)
        Fail: parent(sue,_)
      Fail: ancestor(sue,X)
      Redo: parent(ann,sue)
      Fail: parent(ann,_)
    Fail: ancestor(ann,X)
    Redo: parent(bob,ann)
    Exit: parent(bob,pat)
    Call: ancestor(pat,X)
      Call: parent(pat,X)
      Exit: parent(pat,jim)
    Exit: ancestor(pat,jim)
  Exit: ancestor(bob,jim)
Exit: ancestor(tom,jim)
answer 6: X=jim
Redo: ancestor(tom,jim)
  Redo: ancestor(bob,jim)
    Redo: ancestor(pat,jim)
      Redo: parent(pat,jim)
      Fail: parent(pat,X)
      Call: parent(pat,_)
      Exit: parent(pat,jim)
      Call: ancestor(jim,X)
        Call: parent(jim,X)
        Fail: parent(jim,X)
        Call: parent(jim,_)
        Fail: parent(jim,_)
      Fail: ancestor(jim,X)
      Redo: parent(pat,jim)
      Fail: parent(pat,_)
    Fail: ancestor(pat,X)
    Redo: parent(bob,pat)
    Fail: parent(bob,_)
  Fail: ancestor(bob,X)
  Redo: parent(tom,bob)
  Fail: parent(tom,_)
Fail: ancestor(tom,X)
== 6 answers ==

Every Call line is a push. The first one, Call: ancestor(tom,X), is like calling outer(1) in Activity 15, because the call gets its own new frame. The indentation shows how deep the stack is. Call: parent(tom,X) is indented one step because it runs inside the ancestor(tom,X) frame, just like walk sitting on top of outer. Later, Call: ancestor(bob,X) pushes another ancestor frame, and Call: parent(bob,X) goes one step deeper again. A pop is a fail line, where a goal has run out of options and its frame is removed. Fail: ancestor(liz,X) is one, and so is the last line, Fail: ancestor(tom,X). The exit lines pass an answer back to the caller, like a return, but they don't remove the frame.

In Activity 15, when walk(0) returned, its frame was popped and its locals and return address were thrown away, so nothing could go back into it. A goal that exited in the trace is different. After Exit: ancestor(tom,liz), the trace later shows Redo: ancestor(tom,liz) and then Redo: parent(tom,liz), which ends in Exit: parent(tom,bob). So the goal still remembered which fact it used and that more facts were left. That is the choice point. It works even when the frames are deep. Redo: ancestor(tom,pat) reaches into ancestor(bob,pat) and parent(bob,pat), and each one still remembers where it stopped.


- Why does swapping the two clauses of `reach/2` change whether the search
  ever answers, while swapping the two clauses of `ancestor/2` only changes
  the order of the answers?
