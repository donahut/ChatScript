# Moxie

**Author:** Tom Donahue
**Date:** 4/16/22


## Background
- As soon as I saw the syntax of ChatScript I was immediately struck by how similar it was to [Aldebaran / Softbank Robotics's QiChat](http://doc.aldebaran.com/2-5/naoqi/interaction/dialog/dialog.html#dialog-concepts) that I worked with heavily back in 2014-16(before heading to Jibo).
- At the time I was unaware that it was a fork of ChatScript, but after digging into this assignment a bit, I've since discovered that it was! Cool!

## Set-up
- Upon pulling down the ChatScript repo and following the set-up instruction for running on a Mac, I experienced random segfaults at various points in execution
- There is an open issue for this problem:
  - [Random segfaults · Issue #65 · ChatScript/ChatScript · GitHub](https://github.com/ChatScript/ChatScript/issues/65)
  - The maintainer cut a new release `12.1` that attempts to address it but (1) it doesn't appear to, and (2) there was no binary released for MacOS in that release
  - Another developer suggests that `10.8` does not have this problem, but it too lacked a MacOS release
- Additionally, most `:commands` were broken (in that they printed nothing) in the CLI
- As a workaround, I:
  - Found the last stable release that contained a MacOS binary (`10.2`)
  - This release does not experience the segfaults and the `:commands` all appear to work as expected
  - Created a `moxie` based off of the `10.2` tag

## Testing & Evaluation
  - My repo can be found here: https://github.com/donahut/ChatScript/
    - **Note:** Checkout branch is `moxie`
  - At the root: `./BINARIES/MacChatScript local`
  - In the CLI:
    - `:build 0` -- I changed some filed outside of the HARRY subdir
    - `:build Harry`
    - (`:reset`, when necessary)
 - Code Diff: https://github.com/donahut/ChatScript/compare/10.2...donahut:moxie?expand=1&w=1
    - **Note:** Keep the `w=1` flag tripped in that URL, there were a number of whitespace clean-up changes made by my editor that make it harder to see the substantive additions without
    - **Note:** Each commit is labeled with which Question it is tackling, in case you want to isolate them
---
## Assignments

## Question 1:
- My initial experiementation is limited solely to `introductions.top`
  - After realizing that bot was named after Harry Potter, I wove in some fun from the series

## Question 2:
### Part A
- Modify the file `RAWDATA/WORLDDATA/animals.tbl` where the current tadpole entry (ln 25) lists `tadpole toad` and switch it to  `tadpole [toad frog]`. To ingest this change, I believe you need to run `:build 0` in the CLI which will regenerate the file  `TOPIC/BUILD0/facts0.txt`  with the added line `( tadpole child frog )` .

### Part B
- I'm not 100% sure what was meant by “script command at runtime” in this context, but based on the documentation and `animals.tbl` (where all the relationships are created) I believe the answer may be:
```
^createfact(tadpole child frog)`
```

## Question 3:
- This question gave me the most trouble and I'm quite interested to know how to accomplish this sucessfully
- My `outputmacro` is as follows:
```
outputmacro: ^stringToConcept ($_string)
    ^join( \~ $_string )
```
- From my debugging sessions, I know that it at least creates the concept-esque string `animals` => `~animals` because I stored the result in a $variable and checked the contents of it via `:variables`
- However, validating my approach brings us to the **Bonus**
    - Despite extended tweaks, I’ve yet to be able to utilize this as a concept in a topic. My best guess is that it doesn't work because:
      - (a) Converting a `string` to a `concept` is not equivalent to appending a `~` character to the concept name string, which would mean my entire approach here was invalid.
      - (b) My approach to appending the `~` is valid, however, to utilize it as a concept, there is some problem with encoding, implicit evaluation happening by ChatScript, or there's some set of functions as yet undiscovered by me to make it operate as a concept in a topic.
    - If you're interested at my attempts to use it, it's commented out at the bottom of `favorites.top`

## Question 4:
- Added `favorites.top` that contains an extended conversation around favorite animals and tangents into pets.
- From my experience with Jibo, users love to get to know the bot by asking what it likes and dislikes (and for the bot to remember their likes/dislikes), so that's why I gravitated toward that here.
- At any time, the user can re-enter the "favorite animal" discussion by asking to talk about animals in some way.
- Throughout the dialog, there are various minor tense/case/pluralization mismatches sprinkled about that I regretfully didn't have time to clean-up.
- **Note:** After working on the pet sub-dialog in my topic, I noticed that there’s a similar topic in the ChatScript WIKI under `User Memorization in Conversation`. For what it's worth, I didn’t base my topic off of it.

## Question 5:
- Added `~maybe` concept to `RAWDATA/ONTOLOGY/ENGLISH/concepts.top`
	- In the past, during my time at Aldebaran, we always had topic files like this one where we stored various sets of useful concepts to be re-used across topics, rather than defining them where we were using them (unless they had very limited scope), which is why I took that approach here.
- Added `DONTKNOW` `patternmacro` in `childhood.top`
	- I had a some trouble with using this as a rule in the topic. From my debugging, it seemed to clash with built in Quibbles (which have similar names and matching criteria). As a workaround, I disabled the Quibbles for now. I’m sure there’s a better way to handle such clashes, but I wanted to focus on my additions separately for this exercise given the time constraints.