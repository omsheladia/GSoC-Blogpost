# GSoC 2022 Blog

## The blog contains about all the contributions I made and the progress I made in the course of time till the final-evaluation i.e. September 2022.

# Chapter 0
### Pre-GSoC Phase
#### ??? - 20th May, 2022
It had been days since I participated in a robotics competition where the communication with the robot was made using ssh. And a few days later while going through the announced organisations for GSoC 2022 I stumbled upon libssh. Since I was amazed with this newly found techstack i.e ssh, libssh ignited a spark of curiosity inside me. I went through the projects and found the perfect one for me which opened up a new world for me which is - the world of cryptography. 

I immediately checked out their gitlab repo, looked at the codebase and the project I chose seemed doable. I joined the IRC channel, went through the previous conversations and the libssh community seemed welcoming to all the newcomers making it a good place to start contributing in the OpenSource space.

Going back to the project description, I noted down some of the requirements which were knowledge about elliptical curves and U2F. I started learning about these terms and I can't explain how fascinated I was when I was learning them. It showed some mathematical approaches to the complex algorithms and I started asking myself - when did maths become so interesting? And U2F, I will get to work with one more interesting stuff which could change libssh. All this factors must have inspired whatever work I did till date.

Coming to contribution, I started with a good first issue about implementing a new function which could set a disconnect message. What seemed a simple task was indeed something much more beyond. After pushing some absurd API in the beginning, I realised that even a tiny mistake can wreck the whole system/security. Still trying to understand the codebase, I submitted all the changes told by the mentors and yet I was nowhere near the end.
Managing the end semester examination preparations and my open source contribution, my contributions were a little late.

In the meantime I researched whatever I could about U2F and thought about ways in which I could finish the project and jotted it down in my proposal. And that was a lot of work to get done in a short span of time keeping in mind my end semester exams.

# Chapter 1
### Community Bonding Period
#### 20th May - 12th June, 2022
So for the Community Bonding, there was a lot of rescheduling of meets involved to meet the libssh community together along with the other GSoC student. I got to meet both my mentors Jakub Jelen and Anderson Sasaki, got to know more about them and was impressed with the work they do. I decided to learn from them everything I could. Even though everyone were shy at first, the meeting ended with a great talk from everyone and also some laughs. 

By 27th May my exams got over, and I had decided to go on a short workacation. Hence making time for GSoC was a bit difficult while travelling or when there were powercuts in the remotest parts of the country where I visited.

We decided the flow of the project in this period and also discussing all the aspects required while completing this project. And the foundation had been laid successfully.
Also, the issue I was working on wasn't closed yet, since a lot of technical aspects were considered so as not to breach any security parameters. So I still kept working on the issue and kept on exploring the codebase of libssh. 

All in all, the Community Bonding Period was quite fun! I got to enjoy a bit of my post-exam ???summer??? break, and also spent some time coding.


<h1 align="center"> 
It's Coding Time! 
</h1>


# Chapter 2:
So it was time to implement the flow which was discussed in the meetings during the Community Bonding Period which looked something like this:   
<p align="center">
<img src = "https://user-images.githubusercontent.com/84867886/183337913-e63d5f42-b1e9-4a7d-a001-70c4a3350893.png" />
</p>


From the 2 available options, using existing 'SK' API and implementing something similar in libssh to avail the use of U2F/FIDO seemed feasible.
I started exploring the openssh repository and sk-API in particular and tried to understand what I was getting into. I didn't make any haste while doing so and was thinking of ways in which I could implement all this using the libssh components.

While doing all this, my focus was brought back to the incomplete issue I was trying to solve. We decided to close that issue before the next month's issue, and gave my everything to get it done. Jakub made sure there was no compromise made in the work I was doing, he gave attention to tiny possibilities which could open up big loopholes in the future like memory leak and many other things like writing proper testcases. This made me realise the level of work in the libssh community and I was inspired by that. Finally my request was merged and came along my first official contribution to libssh.

# Chapter 3: 
Coming back to the project, finally it was time to implement stuff I had been gathering information about.
For the sk API, here is a list of files that I meant to include in the libssh
```
sk-api.h
sk-usbhid.c
ssh-sk-client.c
ssh-sk.h
ssh-sk.c
ssh-sk-helper.c
ssh-ecdsa-sk.c
ssh-ed25519-sk.c
PROTOCOL.u2f (For Reference)
```

To build this in libssh, I had to include too many files from Openssh and some of them were necessary. And even while building all this I faced many errors which wouldn't vanish until I applied some weird hack by including a line or a function sometimes or even a definition.
After a long work of doing anything to get it built, I finally succedded just to see this might go in vain.

Right, all the unnecessary header files I included to build the API had to be removed and replaced by the original and existing libssh files/terminologies since this is a project original in itself and just inspired from OpenSSh's sk-API. This seemed easy at first but I wasn't aware where I was getting into.

From 6-7 header files I included in ssh-sk.c from Openssh, I had to remove each one of them except the sk-api.h and ssh-sk.h.
I managed to get some part of it done by trying to find the exact replacements in the libssh repository or writing it in a way to that it throws no error.

Eliminating errors one at a time, I still managed to move further and was left with the last 2 files and the most important one which were the sshbuf.h and sshkey.h .
Both these files took much more efforts than the others combined.

I faced many challenges while doing this. Let me get into this, even though both libssh and Openssh are implement in a similar manner, their API's are somehow similar but also have some differences, and these differences were something that got in my way and gave birth to errors I had to eliminate. 
But I had one thing which helped me win over the errors - the expertise of both my mentors. I would inform my mentors where I was stuck after doing everything from my side. Yeah some of my doubts might have been absurd, but I am in a learning phase, so I try to learn whatever I could about cryptography or even basic coding practices which my mentors feel is a necessary skill.

After replacing a few things, browsing through extensive code lines and putting some things here and there, I was successfully building the required target file.

Here is the output of the progress:  
<p align="center">
<img src = "https://user-images.githubusercontent.com/84867886/183339500-a89c1387-bf15-4dea-b2a1-3a054e652d47.png" />
</p> 

I believe I could I have done a lot more in this span, but this is some great level of achievement for myself and has raised my confidence in a level I didn't imagine in the beginning of the project. 

Here is the link to my forked repository where I am updating all my code: https://gitlab.com/omsheladia/libssh-mirror/-/tree/interface1

# Chapter 4:
After being done with the blogpost I started interfacing the sk-usbhid.c and also gave a glimpse to some of the cryptographic resources shared by Anderson Sasaki. Ofcourse with the errors which were about to come with this file added, I luckily tackled them a bit quickly than before. 
Now was the time to link this "libfido2" library with libssh.
The library seemed to be properly linked on my local system whereas on the remote repository there were some errors in the pipeline.
libssh also has one more repository named "[build-images](https://gitlab.com/libssh/build-images)" which is responsible for the building the images on the CI Pipeline.
I started a merge request and added the steps to install the libfido2 library in various systems and the merge request was closed after a successful pipeline.

# Chapter 5:
Even though the build images was able to successfully install the libfido2 library, while building my repository there were still some errors due to the linking in Cmake.
I made new Cmake modules for the libfido2 and also added a few things in the CMakeLists contained in various folders/sub-folders.
No errors were being thrown on my local system but that was not the case on the remote system. This became really difficult to debug as I had to push something everytime I made some changes and had to wait for the CI to finish running the pipelines which took a lot of time.
<p align="center">
<img src ="https://www.incredibuild.com/wp-content/uploads/2022/03/C-Meme.png" height="300" width = "200">
</p>

After trying everything I could, I discussed it in the meeting and I was suggested to try one more thing.
Trying that thing didn't work but making some few more changes in it and after giving my brain that hard time I was able to eliminate that linking error.
This gave me a sense of relief that the libfido2 library was properly linked with the libssh.

# Chapter 6:
Now after this important part being achieved, I had to move on with the authentication part.
Unfortunately I fell sick for a week which impacted my progress a lot.
After getting a little better I started working again on the authentication part and I pushed some part. There were no errors thrown on my local system but that was not the case with the remote system.
I was not sure about the part that I pushed and there was no way to test it. Yet I tried solving the pipeline errors thinking it might give away some clue to where I might be wrong. A lot of time was spent in this part yet I was trying my best till the end.
By this time my number of commits had been increased very much. I squashed those commits and then again started to fix the pipeline.
After spending a lot of time I was able to fix pipeline, by fixing 1 test part of the pipeline at a time.
Since at this point there were no errors, I created a merge request.

# Chapter 7:
The authentication part I did earlier didn't seem right to me. So I scrapped that approach down and also fixed some part of the code on the review I received on the merge request.
Since I was trying to use the sk API from OpenSSH, the difference in both of their API stood in my way.
Even to add a single line I had to browse through hundreds of line just to ensure if I was doing it right. 
I was caught in this loop where I was trying everything I could and there were no positive results and it felt like I made it worse.
After all this endless trying, during my last meeting when my mentors had also lost the hope but I wanted to complete this project at any cost. They still gave me a chance till the evaluation.
So in these last 3 days I pulled out 2 all nighters and came up with something better than the last work only problem was that it showed pipeline error in a testcase I created. 
Though this was some of my best work I couldn't pass the evaluation.
But as I had promised my mentors that I would complete this project irrespective of the evaluation, I have started working on it again and hope to give them a positive result in the weeks to come.
