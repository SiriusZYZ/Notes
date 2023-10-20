
# self-introduction

Hi, interviewer! It's my pleasure to take part in today's interview.
My name is Yinze Zhou. I am currently studying as a postgraduate student in the University of Electronic Science and Technology of China , and I am about to graduate in twenty-twenty-four. 
My major is electronic information, which is pretty much generalized. What I'm actually studying is the application of distributed fiber optic sensing in oilfield monitoring and production optimization.   
During these year of study, I have many opportunities to combine petroleum science and information technology in my research, including parameters adjusting, modeling, visualization, inversion, and so on. I also have several project experience which are related to software development.
I knew slb when I was the T.A. of my supervisor's class, and joined the intern day event a few weeks ago. And found myself very interested in the position of software engineering. 
That's all of my introduction.

### who is your supervisor?
- He is the chair of SPWLA south western china chapter. His research focus is acoustic behavior in the wellbore, especially on the completion of the wellbore.

# work location: why Beijing
- Well I guess the biggest reason is Beijing is a city of opportunities. The headquarters of a lot of companies locate in Beijing. And I know BGC is one of the biggest software development center of SLB. So it's possible to work with many brilliant people across the whole county and even the whole world. And that attracts me most.
- I also have some friend in Beijing. Some of them are still in collage, some of them have already been working. The point is I have a social support system in Beijing. Shanghai is also a city of opportunities, but I have no friend there so Shanghai is not a good choice.
- My parents are also supportive about I work in Beijing the whole stuff. 


# SLB background

# What do you know about SLB?
- Background:
  1. founded by Schlumberger Brothers, in 20
- Business scope
  - oil field service
  - decarbonization
  - innovation energy

# how do you know SLB?
- back in twenty twenty-two, I was the TA of my professor. He was teaching a lesson called Exploration Technology and Engineering. In a certain lesson, he mentioned the resistivity logging, and Schlumberger's Brothers. After the class, he sent an article regarding to slb, and I found out slb is actually the biggest oil field service company in the world.
- Later on, one of the engineer of slb came to our school to give a lecture. Her name is Tianhua as I recall. Her lecture was about the oil field service industry, and focused on slb's new business. And by then I knew the Beijing Geoscience Center of slb, and your digitalization and decarbonization business. 
- In August, I came to Beijing to participate in the Inter Day Event, then I understand SLB better. 
# Why SLB?
There are some certain aspects when I wanna decide which company suit me most.
1. The first thing would be the position of the company in the industry. SLB is the biggest company in oil field service. I also got to know that SLB's decarbonization business in Europe is massive. These would be the guarantee of a secure job, as the company would gain enough profit to sustain its R&D department. 
2. The Second one would be the job itself. I am applying for the software engineering, which is the job I always dream of. I like coding, implementing stuff and find out how to optimize them. I learned that in BGC, people cares about techs and there are a lot of competition, Hackathons for example.  I believe I could learn a lot in this position.
3. The third thing would be the company's culture, and I think that is what I love SLB a lot. 
   - SLB cares about safety of its employees. You have rigorous safety protocols for field engineers and mechanics engineers , you have safety training for personnel who work in the office. But I want to mention something that touch me a lot.  I read an article about the first aid day on your WeChat public account. It's about CPR and AED equipment training,  what to do when someone is choking, and other first aid knowledge. I was really impressed because I always want to learn how to properly perform the CPR back in the school. And I think learning first aid knowledge not only benefit the employee themselves, but also benefit to the whole society.
  - BGC also has a diverse and inclusive culture. Which means one can learn a lot from others with different background. And I think that would benefit in both the professional way and the daily life way. 
  - And I found people in BGC have a sense of ownership, and the company cares about employee's innovative ideas. In the campus recruiting intro video clip, I learned that some of your products fancy features are origin from the employees' inspiration. At first, it was those software engineers who came up with some new idea and try to demonstrate on the innovation competition. Then SLB found it feasible and decided to integrate this feature into the products . Which I guess, would give those engineer a sense of achievement and ownership of SLB. And I think that's very nice.
  

# Project
## Borehole acoustic evaluation project.

### difficulties

- So this is a project that party A requires us to  deliver software module in Cpp, but our source code are written in Matlab.
- the biggest difficulties came when a part of my job was to translate a certain matlab built-in function. You need to find how this function works, check if anyone overhaul them in other programing language. And try to implement one in your cpp module.
- So I got a built-in function called variational mode decomposition, also known as VMD. It's a function for signal decomposition. It's a new method so we havn't seen many people using it. So I checked the paper which introduce vmd, found their source code written in Matlab. I also found a cpp implementation in Github. But they takes up too many memory space. So I carefully go through their code and found the unnecessary and buggy part. Then I refactored the whole function to make it more efficient and reusable.

### details
 - It's a consignment from an state-owned oil filed company. The whole thing was about determination of the data quality of borehole acoustic instrument.
 - We have a team of 8 people, 2 of the team contributed the Matlab code, and the rest of us port them and the dependencies into cpp.
 -  the biggest difficulties came when a part of my job was to translate a certain matlab built-in function. You need to find how this function works, check if anyone overhaul them in other programing language. And try to implement one in your cpp module.
- So I got a built-in function called variational mode decomposition, also known as VMD. It's a function for signal decomposition and it's crucial because it's called by many other function. It's a new method so we haven't seen many people using it. So I checked the paper which introduce VMD, found their Matlabsource code. I also found a cpp implementation in Github. But they takes up too many memory space. So I carefully go through their code, do some proofreading job, and found the unnecessary and buggy part. Then I refactored the whole function to make it more efficient and reusable.


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


## DFOS project

> Field experience
> So our team has a DTS equipment, and a friend of my mentor has a DAS equipment. They somehow got in touch with a oil field service company in 克拉玛依 in twenty-twenty-one. The FEs want to see if DFOS could help them estimate the main production section in some heavy oil well. The logging was done by lower a continuous tubing into the auxiliary pipe. We used the DTS and the DAS to monitor the well when they change production schema. 
> In the first day, we check our equipment carefully to see if they work well, we also do some safety training. The other day we went to the oil field to actually log some data. We finished one well once a day.

> web crawling
> The key problem of the whole project is to establish a thermodynamic model of the fluid, the wellbore and the formation. Which, naturally, you need information of the fluid behavior, such as its density, viscosity, heat capacity and so on. And this parameters change due to the temperature and pressure. I happened to find that a website called NIST, they provide thermodynamic properties of different compounds. So I tried to fetch a small amount of data by set up a automation script in Python to query`[ˈkwɪəri]` their public API repeatedly. 


> Conclusion / details
- To be brief, the whole project is about how to estimate the flow condition in the wellbore using data from Distributed Fiber Optic Sensor. To be specific, we want to know the flow rate, gas oil ratio, or GOR, water cut, and so on. The data we have are from Distributed Temperature Sensing, also known as DTS, and Distributed Acoustic Sensing, or DAS. The first one provide you with the temperature distribution along the wellbore, while the latter one provide the dynamic strain rate distribution.
- To solve this problem, we need to combine these data with the flow model and the thermodynamic model. We also need some signal processing technique to understand DAS data better.
- The most difficult part of this project, would be the lack of data. We can not solve the problem without other constrain, such as the well completion, the formation or reservoir information. We also need the PLT data to validate if our model works correctly. 
- This kind of difficulty could be solved by cooperate with oil field service company or the oil field owner. We are also working on establishing a simulation process using finite element model, to do some experiment.
- This project is also challenging in modeling, parameters calculation, inversion process, and other way. But I managed to solve them already. Anyway, I hope the data problem would be solved very soon.

## Challenge Cup

> difficulties
> the biggest difficulties was the tight schedule. I was informed that I need to establish the whole analysis process within a week, while I have no clues on how seismic data processing work.
> So I reached out to my mentor, who has the background knowledge. He gave me some keywords and a few books to refer to. And reading articles and those books, it came with an Eureka moment. Then I started to program to find out if the methods worked.

> experiment plan?
> We were thinking about using some iron ball to simulate the source.

> detail
- To be brief, the whole project was the idea of my mentor and another professor from the transmission school. They developed a DAS instrument, but want to apply it for seismic `[ˈsaɪzmɪk]` exploration. My mentor has the background of seismic signal processing, and I have the background of Distributed sensor. So we form a team, to participate in a national competition.
- My job include gathering information, assisting the experiment, establishing a data analysis process, and some document writing.
- But actually I don't know anything about seismic signal processing at that time, and the schedule was tight and I need to establish a processing procedure for DAS data. So I reached out to my mentor and a classmate, to better understand how to extract useful information from the array signal. With their help, I learnt how to process the DAS signal to calculate the dispersion curve using the MASW method. And find a way to estimate the velocity model from it. 
- In the end the team won the first price of the provincial competition.
# Profile

## advantages and disadvantages?

for advantages:
- I have strong learning ability, which means I can learn things fast and well, even through self-learning. I kind of learn by myself in most of my curriculum in a very short time, but my test scores are high. My GPA is 3.7 and it's the top 10 percent in my major.
- I have multi-tasking skills. As a postgraduate student looking for a job, I need to balance the time I spend on self-promoting, academic research, and some of the project. But I handle them quite well. I would have a regular routine to divide my time properly to make progress on different tasks. My timetable is also flexible, in case there is a contingency.
- I am able to collaborate with others quite well and I could blend in the new group quickly. In one of my project experience, I don't know any of my teammates at first. But after a few round of discussion, I manage to blend in the team and collaborate well with them. I even keep in touch with one of the teammate after the project is done, and sometimes we exercise together in the campus.
- I am also a caring person according to my friends. Many of them would reach out to me if they are upset, and I would listen carefully to them and cheer them up.

disadvantages:
- Sometimes I get a little bit nervous when something important is about to take place. For example, if I am about to give an opening speech in front of many people, I would feel tense the night before. But l read a psychology article about anxiety and other negative mood, and it said anxiety is a mood that urge you to prepare for the upcoming difficulties, and it's a normal reaction. So whenever I feel anxious, I would  try to do something and get more prepared. I guess you can't actually eliminate anxiety but that helps me a lot.
- 【先别说】Sometimes I am not assertive enough. It takes a little bit more time for me to make a decision and I want to hear more about others opinion. But this happens more when the decision is insignificant and about daily life. Like I often have no clue about what to eat when there are a lot of alternatives. But when the decision is significant, like which company is my top choice, whether I should get a master degree, What to do in an emergency,  I seldom hesitate.
## biggest achievement?

- I think my biggest achievement for now, is that I balance my life very well in the postgraduate study. I learn a lot during my research on DFOS application. I also learn a lot about programming and contribute to the open-source community.
- I also manage to make a bunch of good friends during this period of time, they are from different school and we are having a good time.
- I also read a lot about psychology, history and other subject beyond my scope. 


## biggest failure?

- I didn't do well in the collage entrance exam a few years ago. I did well in senior high school, and I got an expectation to go to Fu Dan university, or at least Sun-Yat-Sen university. But I end up in UESTC.  
- But I guess a blessing in disguise is a blessing in disguise, I adapted myself to the life in UESTC quickly, and found myself interested in my bachelor major, which was actually a combination of computer science, signal processing and geoscience. 



## A word to describe yourself?
flexibility?
- I think I am flexible in many ways. I can adapt to the new environment quickly. I

## collage life

- I think my collage life is full of challenges, but also fruitful in many ways.  
  - **==Curriculum:==** The curriculums are not easy to pass, most of them requires a lot of practice, but I managed to pass them all and have a good GPA, and received the first class of  scholarship .
  - **==Research:==** My research direction is also quite hard. There are tons of unexpected problems, from the availability of the data to the validation process.   It's also a new direction of the research team, none of us has the background on flow profiling, including my mentor. But after 2 years of self-study, and propelling the process with others. We have got some data, although they are not comprehensive enough, but it's a good start. I also develop some forward process and inversion process. I am also establishing codes for the simulation process which, after my graduation, the other may use them as a complement of field experiment.
  - **==workshop:==** I also enjoyed a lot in the extra-curriculum activities. I joined a school-affiliated workshop when I was at my first year in the UESTC. So it has been like 5 years of service. We write articles of interesting stuff around the campus. I also made a lot of friends there. We are from a vast variety of majors, so we got inspired from others perspective and knowledge a lot. 
  - **==Interest:==** I also maintain good health in the university, I swim a lot, and sometimes I will play badminton with my fellow teammates. I also develop an interest of photography, and cocktail making.
  - So I would conclude my university life as challenging but fruitful.

## what do you see yourself in five year?

- Hopefully, I wish I have learned a lot during the following five years, and be able to be a full-stack developer. I also picture my self staying at BGC, be an important member of the team, and make a lot of friends here. I also wish I could contribute to the open source community. I balance my work and life well and keep developing my interest.
- I also hope that I can learn some management skill at BGC, and be able to do some management job.