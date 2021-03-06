# Quantitative ecology primer

## Class one: Introduction

This primer material provides aims to refresh your memory of some of the basic statistical techniques and concepts you have probably been taught previously as an undergraduate. Taking the time to revise some basic stats will help to ensure smooth progress towards understanding and using the more advanced techniques that will be taught later in the course. If you have never learned any statistics at all you will need to carry out quite a lot of self directed reading and learning in order to develop this basic knowledge. In this case you should use the primer material as a guide to the level that is expected at the start of the course. We will look at more subtle issues surrounding the practical application of these hopefully already familiar techniques during the taught sessions at the start of the course. 

The example analyses included in the primer are all directly relevant to the final assignment. Some of the methods shown in the primer documents can be used in the final report, although you may also wish to use the more advanced techniques that will be taught next term where these are most appropriate. Important statistical issues that you will need to consider when analysing any data are introduced in this text.

 These tests are part of the continuous assessment component of this unit which is worth 30\% of the final mark. The tests will assess your understanding of key concepts and the ability to work with data using the methods shown in the primer. Each of the tests will not take more than an hour to complete, providing that the relevant material has been worked through previously.


## Set reading

In addition to these notes you   obtain copies of the text books that provide the set reading for this part of the course. The library has sufficient print and electronic editions. Each class in the Primer will include set chapters from these texts.

Dytham, C. (2011). Choosing and using statistics: a biologists guide. Chichester: Blackwell Science. 

Crawley, M. J. (2005). Statistics: An introduction using R. Hoboken, NJ, USA: John Wiley \& Sons

Dytham's book is a general introduction to statistical methods for biologists/ecologists with an emphasis on statistical hypothesis testing. Crawley introduces the more advanced model based approach to analysis that we will look at in more detail next term.

The data for Crawley's book are available here
http://tinyurl.com/QEcol2013/Crawley.zip
  or from here 
  http://www3.imperial.ac.uk/naturalsciences/research/statisticsusingr
  
## Software

There is a very good case to be made for gaining familiarity with more than one statistical package. Each has its pros and cons and strengths and weaknesses. In the course you will use two of the most popular statistical programs with rather different characteristics. You should use both in order to complete the work in the primer.


### R

The analyses that are shown in the main text of the primer use the open source statistical language R. In recent years this has become the `` '' for environmental data analysis. Ecologists and environmental scientists have developed many specialist packages for R that allow methods that they develop to be used by others. You may wish to look at some articles on these that are published in the BES journal Methods in Ecology and Evolution in order to get a feel for the way researchers are using the software.

 

R is used throughout this document in order to illustrate statistical concepts by breaking down the analyses into steps. Once you have become comfortable with some simple R commands and concepts you will find that this helps you to follow a structured approach to data analysis. There are a very large number of books that use R to teach ecological analysis and concepts. Familiarity with R will allow you to apply an almost unlimited number of analytical techniques within a single common framework. Scripting analyses in R forms part of the widespread drive towards ``reproducible research''. The R commands that are included with the results allow anyone to reproduce the analysis exactly as it was run. This is a very useful way of collaborating with other researchers.

The basic R console is a minimalist interface into which you must type commands to produce results.
 
![alt text](primerfigs/R1.png)
   

There are few menu options and the interface is hardly user friendly. Fortunately, you are not expected to use R independently for your own analyses at this stage. In order to work independently you will need to build up a basic vocabulary of R commands. Finding out how to get R to produce the results you want is not easy at first. The learning curve for R is steep for everyone. 

The good news is that you have been provided with worked examples showing every single command that you need. You do not need to go beyond these basic commands at this stage. It is straitforward to follow the steps by copying the code carefully. Think carefully about what is happening as you do this. The R way of doing things will gradually become more familiar during next term when the formal teaching begins. The analyses in this text all form prebuilt templates for your own work. Simply substituting a new set of data for those used in the example will allow you to rerun the whole analysis and get comparable results. Re-running prewritten code is often faster and simpler than pointing and clicking on a menu. 

You can install R for free from 
http://cran.r-project.org/

You may want to look at the extensive documentation on R available from this site.

### RStudio

A more sophisticated interface for R is provided by RStudio. Rstudio helps to organise your work, allowing you to compile reports using markdown language that combines text, R commands and R output. The documentation for this course has been produced using it. Figure 2 shows some of the features of the RStudio interface. RStudio is not a menu based GUI. However it does make working with R smoother and more polished in many respects. The advantages of RStudio will not be apparent until you have learnt the R basics, so you should first use the console version before moving onto RStudio. I will provide support on using RStudio next term.

   
![alt text](primerfigs/Rstudio.png)

RStudio is available for your laptop from  
http://rstudio.org/

The site includes a demonstration video that shows RStudio in action.


### RCmdr/BiodiversityR

R can also be run using a ``point and click'' interface shown in Figure 3. This is not as complete nor is it as easy to use as SPSS. It can however make running some analyses easier by writing the code for you. You will not be expected to look at this until next term. With a little experience BiodiversityR can be a very useful tool for intermediate level users of R.

![alt text](primerfigs/BioR.png)

![alt text](primerfigs/R.png)

### SPSS

SPSS has a complete and sophisticated Graphical User Interface (GUI) that allows you to run analyses by simply dragging and dropping variables into place as shown in Figure 4. 

![alt text](primerfigs/SPSS.png)
   
 
 Behind the scenes SPSS works much like R. SPSS runs a set of commands over the data and reports the results. However, SPSS automates many of the steps in one pass. This can often make it much quicker to obtain results. Some of the decisions are made for you and additional, sometimes useful, options are added during the analysis. This helps when you are learning stats for the first time. Although SPSS is very useful for conventional analyses it is much more limited than R and cannot run more complex analysis specifically designed for ecological data.

The most difficult part of using SPSS is deciding on which menu option to choose in the first place. Unlike some computer software there is little point in randomly experimenting with menu options in order to see what happens. If you do that you will simply end up running inappropriate analyses that return incomprehensible results. The big advantage of the GUI approach is that, providing that you do know what you want to do, there is no need to remember (nor to type in) all the steps in order to run an analysis. However you do still have to know what you need before you start. SPSS does not, and cannot, choose the analysis for you. Furthermore SPSS output can be very difficult, if not impossible, to interpret if you do not understand the theory that produced it. It is easier to understand what is being calculated by breaking the analysis down bit by bit. This is the strength of the R approach, especially when you are trying to understand what is going on for the first time.

So, in the primer I will show the steps behind the analyses first using R code rather than SPSS. Don't be alarmed by this. All the commands are very simple and you can run them simply by copying the code from the document. You may then wish to run complete analyses in one or two steps using SPSS. After working through the steps in R you will already understand what the SPSS output means and you should know why you chose the analysis. I have included screenshots at the end of each section of the primer showing how to produce identical results in SPSS for all the analyses shown in R. The instructions in Dytham's book are also very useful. Dytham shows how to run each analysis in Excel, SPSS and R. You should look at both my notes and the appropriate chapter of Dytham.

Both SPSS and R carry out exactly the same calculations and will therefore produce exactly the same results for all standard analysis. Simple figures and graphs are quick and easy to produce using R commands, as you will see as you work through the material. This makes R very useful for exploratory data analysis and visualising data structure. R can also produce extremely sophisticated customised output of publication quality. You may want to look at the gallery shown on this site   in order to get an idea of what R can do. However it does some familiarity with the language to produce high quality figures. Customising figures for a final report is often simpler in SPPS, where the figure axes and captions can be edited easily using the mouse. It is worth taking advantage of the best features of both pieces of software.


### Excel

Dytham's book also provides instructions for running statistical analyses in Excel. I do not recommend Excel as a tool for complex statistical analysis. However if you are comfortable with spreadsheets you may find them useful at the start. In Excel you design your analysis by combining functions. This is in effect a similar approach to R. You have to make a lot of your own decisions and know how to use Excel functions and pivot tables. SPSS can usually compute your stats for you much more quickly than Excel and produces more appropriate statistical figures.

All data provided for the course is in CSV (comma separated variable) format. The files can therefore be opened in Excel for editing and manipulation. Excel may be useful in the early stages of the course if you are used to using it.


### Equations

I have provided equations for some of the commonest statistical calculations. Don't be alarmed by them if you do not like maths. This does not imply that the rest of the course will contain even more complex equations and calculations. When we look at the more advanced methods there will be a tendency to leave all the calculations to the computer. However, the baseline equations that define common statistical concepts are well worth understanding in some depth before you go on to carry out more complex analysis. In order to help you to see how the equations work all the calculations have been broken down into steps using R. This may seem intimidating when first glancing through the primer. However if you take the time to work through the material a step at a time it will make sense. Think of the R commands as working versions of the mathematical equations that allow you to quickly implement them with real data. Crawley uses a similar approach. 

I will not duplicate all the more detailed explanations of these concepts that can be found by reading Crawley's introduction to statistics and Dytham's guide. My notes contain the bare bones of the material and some simple R commands that will allow you to run the analyses yourself. You should aim to have a working knowledge of the common statistical procedures before the beginning of next term through working through both the examples in the primer and the relevant chapters in Dytham and Crawley.


### Running R commands

You will receive hands on guidance to using R in the taught part of the course. However it is important to gain some familiarity with R before next term. You can install R on your own laptop free of charge by clicking here  . R is extended through adding packages, but the primer uses only ``base'' functions at the start. You may want to try installing additional packages before the course begins. In particular the package vegan and the BiodiversityR GUI will be used extensively later in the course. There is a vast amount of online material about R. If you are curious about what R can do a quick ``Google'' will provide a lot of examples. R is one of the few single letter search terms that provides a useful result. The next class includes a tutorial to get you started in R. The initial challenge is becoming comfortable with using a command line. This takes time, so do not worry if you find it a bit strange at the start. 


### Running SPSS analyses

The primer includes screenshots of the  . This will only confuse you by producing meaningless output. The only exception to this rule is the SPSS chart builder. It is well worth experimenting with as many of the options for building and editing figures as possible. The interface is quite intuitive, once you get used to it. Because you are producing visual output it is usually easy to understand what you have done. You can still produce meaningless figures this way, but it is usually obvious when they do not make sense. SPSS has an extensive help system which can provide guidance if you get stuck.

### Planning an analysis

The sort of quantitative ecology that you will learn about on this course is all about numerical data. Every research project collects it, and every dissertation contains it. The difference between an excellent dissertation and a weak piece of work often lies in the quality of the data analysis. If you read the first chapter of Dytham you will have seen this suggestion.

 “Collect dummy data (i.e. make up approximate values based on what you expect to obtain). The collection of ‘dummy data’ may seem strange but it will convert the proposed experimental design or sampling routine into something more tangible. The process can often expose flaws or weaknesses in the data- collection routine that will save a huge amount of time and effort.”

“I implore you to use this. I have seen countless students who have spent a long time and a lot of effort collecting data only to find that the experimental or sampling design was not quite right. The test they are forced to use is much less powerful than one they could have used with only a slight change in the experimental design. This sort experience tends to turn people away from statistics and become ‘scared’ of them. This is a great shame as statistics are a hugely useful and vital tool in science.”

 

I strongly agree with this author. I have had exactly the same experience. Year after year I have been asked by students to help them analyse their data only to find that they did not plan the data collection with any particular analysis in mind. This never works. Not only is the analysis not likely to show much, but the data will rarely be structured in a manner that allows an analysis to be run at all! This leads to a pre-write up panic and post write-up anguish about what might have been.

So, data collection and data analysis   be planned together. There is no realistic alternative. If you understand how an analysis works you will also understand what sort of data are needed to run it. As you are still learning the basics of quantitative ecology there is clearly something of a chicken and egg situation here. How can you know which analysis you are going to need if you still have not learned enough to choose from? This is a valid point. Nevertheless there are some simple concepts that you can use that avoid many problems later on. 

The steps for any successful research project which has a quantitative component are;
 
*  Decide on one or more interesting and tractable scientific questions.
*  Design an experiment or field survey that will produce data that addresses the question(s).
*  Decide how the data you will collect relates to your scientific question(s). This step will involve re-framing some or all of the questions as statistical hypotheses and/or statistical models.
*  Decide on the planned analysis that you wish to conduct on your data. These should effectively summarise key features, describe patterns, test hypotheses, establish associations between variables and/or estimate effect sizes and parameter values. 
*  Test your analysis carefully (preferably using dummy data) to ensure that you know how to run them. Critically evaluate whether your field work is likely to provide suitable data (sufficiently replicated) in a suitable format. Make contingency plans and decide on alternative analysis in case data do not meet the assumptions needed for the planned analysis.
*  Collect the data.
*  Investigate the properties of the data you have collected in the light of the assumptions used in your analysis. 
*  Run the planned analysis (or the contingency analysis) and obtain the appropriate data summaries, test results, model output, tables and figures.
*  Interpret and discuss the results.
 
Although this course concentrates on the analytical component of this research sequence, the initial steps rely on knowledge of the specific system you are studying. Quantitative ecology allows you to apply powerful computational tools to your own research, but these tools will only be effective if the right questions are formulated from the start. It is often stated that the best questions are the simplest because they lead easily into the formulation of clear hypotheses or models. This is good advice. However ecological research inevitably deals with complex systems involving multiple variables. The most appropriate analysis for MSc research projects can involve computational methods that go beyond the statistical tests found in introductory texts. Without a working knowledge of the available techniques it can be difficult to extract all the information from your data and link the results to the motivating questions.


### Reading for this week

The preface and chapters 1 and 2 of Dytham (2011). This should provide a good overview of some key concepts involved in designing studies and analyses. You should be aware that researchers can place different emphases on some aspects of study design and statistical analysis. There is no single ``correct'' way to proceed, so read both this week's texts critically.

Chapter 1 of Crawley (2007). This is heavier going than Dytham, but more comprehensive and includes more theory. You do not need to understand all the concepts mentioned at this point. However Crawley introduces many important ideas that are central to this course including model building that we will see next term. Notice how Crawley illustrates ideas with snippets of R code. You will begin using R in the next class and you may wish to start trying Crawley's code then.

 
