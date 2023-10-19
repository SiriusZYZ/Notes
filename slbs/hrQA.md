
# self-introduction

Hi, interviewer! It's my pleasure to take part in today's interview.
My name is Zhou Yinze. I am currently studying as a postgraduate student in the University of Electronic Science and Technology of China , and I am about to graduate in twenty-twenty-four. 
My major is electronic information, which is pretty much generalized. What I'm actually studying is the application of distributed fiber optic sensing in oilfield monitoring and production optimization.   
During these year of study, I have many opportunities to combine petroleum science and information technology in my research, including parameters adjusting, modeling, visualization, inversion, and so on. I also have several project experience which are related to software development.
I knew slb when I was the T.A. of my supervisor's class, and joined the intern day event a few weeks ago. And found myself very interested in the position of software engineering. 
That's all of my introduction.

### who is your supervisor?
- He is the chair of SPWLA south western china chapter. His research focus is acoustic behavior in the wellbore, especially on the completion of the wellbore.


# SLB background

# What do you know about SLB?
- Background:
  1. founded by Schlumberger Brothers, in 20
- Business scope
  - oil field service
  -
# how do you know SLB?
- back in twenty twenty-two, I was the TA of my professor. He was teaching a lesson called Exploration Technology and Engineering. In a certain lesson, he mentioned the resistivity logging, and Schlumberger's Brothers. After the class, he sent an article regarding to slb, and I found out slb is actually the biggest oil field service company in the world.
- Later on, one of the engineer of slb came to our school to give a lecture. Her name is Tianhua as I recall. Her lecture was about the oil field service industry, and focused on slb's pivot. And by then I knew the Beijing Geoscience Center of slb, and your digitalization and decarbonization business. 
- In August, I came to Beijing to participate in the Inter Day Event, then I understand SLB better. 
# Why SLB?
There are some certain aspects when I wanna decide which company suit me most.
1. The first thing would be the position of the company in the industry. SLB is the biggest company in oil field service. During the Inter day event, I got to know that SLB's decarbonization business in Europe is massive. So you are not merely focusing on the petroleum, which is nice. These would be the guarantee of a secure job, as the company would gain enough profit to sustain its R&D department. 
2. The Second one would be the job itself. I am applying for the software engineering, which is the job I always dream of. I like coding, implementing stuff and find out how to optimize them. I learned that in BGC, people cares about techs and there are a lot of competition, Hackathons for example. 
3. Then I also want to check the platform and how it may impact my whole career planning. So during the Intern Day, I got to know that you guys have a concept of " borderless-career". It's possible to explore different positions when you are at SLB. And SLB provide comprehensive trainings to help you pivot, which is great. 
4. The third thing would be the company's culture, and I think that is what I love SLB a lot. 
   - SLB cares about safety of its employees. You have rigorous safety protocols for field engineers and mechanics engineers , you have safety training for personnel who work in the office and so on. But I want to mention something that touch me a lot.  I read an article about the first aid day on your WeChat public account. It's about CPR and AED equipment training,  what to do when someone is choking, and other first aid knowledge. I was really impressed because I always want to learn how to properly perform the CPR back in the school. But they never open a lecture for first aid and that's a pity.
  - SLB also have a diverse and inclusive culture. Which means one can learn a lot from others with different background. And I think that would benefit in both the professional way and the daily life way. 
  - And I found people in SLB have a sense of ownership. In the propaganda video clip, I learned that some of your products fancy features are origin from the employees' inspiration. At first, it was those software engineers who came up with some new idea and try to demonstrate on some kind of innovation competition. Then SLB found it plausible and decided to integrate them into the products as a new feature. Which i guess, would give those engineer a sense of achievement and ownership of SLB.

# Project
## Borehole acoustic evaluation project.

### difficulties

- So this is a project that party A requires us to  deliver software module in Cpp, but our source code are written in Matlab.
- the biggest difficulties would be the part when you don't know the source code of a certain matlab built-in function . You need to find how this function works, check if anyone have overwrite that function in cpp, well actually any language will do, because the source code matters.
- I got a function called vmd, it's the variational mode decompostion. 

### details
 - a consignment from an state-owned oil filed company. 
 - determine the data quality of borehole acoustic instrument
 - a team of 8 ppl
   1. Matlab 2, which is the original implementation of the algorithms 
   2. rest of us : Cpp
 - Why CPP: these state-owned company wants to guarantee their software products would not contain any incontrollable foreign tech, matlab for example.
 - We need to port every functions the Matlab crew wrote, along with the some of the Matlab built-in functions.
 - among them, there is a function called vmd, which is the variational mode decomposition, and it has been called by more than 20 other function, which makes it vital for the whole project.
 - Process:
   1. At first I want to get the Matlab source code of it, but failed. 
   2. Then i turn to the academic society, and found out that vmd was introduced in 2014 in an IEEE paper. In this paper, the authors attached the original source code of vmd in Matlab. And the script is open-source with an MIT license.  
   3. The first thought come to my head was translating them as usual, but then i think why dont i look it up on github, see if anyone has implement it using cpp. And I did find one, the code was published by a guy called Hugo, he mentioned his implementation was somehow, slow and laggy.
   4. So I tested that code, and found out that it took up too much memory, we were seeing an consumption of up to 4 giga bytes. which is totally unacceptable. But, still the answer is correct.
   5. So there must be some kind of problem with the code. Maybe it doesn't free a pointer, or duplicate something repeatedly. To address the problem, I read the code very carefully (Both the original Matlab code and the Hugo's implementation) and found the problem. 
   6. Vmd is done by iteration to minimize a certain loss function. And each iteration would takes the result of last iteration as part of the input. The result we are talking about is an array, which has the same size as the signal to be decomposed.
   7. So if you want to save results of all the iteration for debugging, and your input is so long. That would be a huge problem. And this is the feature of the original Matlab code, and so does Hugo's code. Maybe he didn't notice that when he was porting from Matlab.
   8. So the solution is clear and simple. As the iteration only use the result of last iteration, we only keep the last one. When the new result comes out, we override the last result. By doing this, we can save a huge space, the times we save is depend on how many iteration are done in the process. And in some cases, we are seeing a reduction of memory usage by more than 100 times.
   9. After I refactored the code, the problem was solved. And I also publish it on github if someone need it.


# Profile

## collage life

- I think my collage life is full of challenges, but also fruitful in many ways.  
  - **==Curriculum:==** The curriculums are not easy to pass, most of them requires a lot of practice, but I managed to pass them all and have a good GPA, and received the first class of  scholarship .
  - **==Research:==** My research direction is also quite hard. There are tons of unexpected problems, from the availability of the data to the validation process.   It's also a new direction of the research team, none of us has the background on flow profiling, including my mentor. But after 2 years of self-study, and propelling the process with others. We have got some data, although they are not comprehensive enough, but it's a good start. I also develop some forward process and inversion process. I am also establishing codes for the simulation process which, after my graduation, the other may use them as a complement of field experiment.
  - **==workshop:==** I also enjoyed a lot in the extra-curriculum activities. I joined a school-affiliated workshop when I was at my first year in the UESTC. So it has been like 5 years of service. We write articles of interesting stuff around the campus. I also made a lot of friends there. We are from a vast variety of majors, so we got inspired from others perspective and knowledge a lot. 
  - **==Interest:==** I also maintain good health in the university, I swim a lot, and sometimes I will play badminton with my fellow teammates. I also develop an interest of photography, and cocktail making.
  - So I would conclude my university life as challengeable but fruitful.

## what do you see yourself in five year?

- Hopefully, I wish I have learned a lot during the following five years, and be able to be a full-stack developer. I also picture my self staying at BGC, be an important member of the team, and make a lot of friends here. I also wish I could contribute to the open source community. I balance my work and life well and keep developing my interest.
- I also hope that I can learn some management skill at BGC, and be able to do some management job.