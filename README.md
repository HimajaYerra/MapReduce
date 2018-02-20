# MapReduce

This is an Eclipse (and Maven) project for implementing google's page_ranking algorithm to define the page rank score of articles(nodes). In this project, we are using mapreduce framework for implementing the above which is defines as PR(u)=0.15 + 0.85 * Sum(PR(v)/L(v)), ∀v: ∃(v,u) ∈S, where L(v) is the number of out-links of page v.

# Compiling and Testing from the Command Line:

This can be run by calling jar_path Class_name input_file no.ofiterations
From the command line, in this directory, type:

mvn package

This will create a jar file and this can be used to set HADOOP_CLASSPATH 

The mainClass here is "MapReduce" and output path & no.of itrations can be set

# Requirements:

Eclipse
Maven
HDFS

#  BigData CourseWork:

There are threee mapreduce jobs configured in the project and the details of KEY-IN, VALUE-IN & KEY-OUT, VALUE-OUT are described below for each job in detail:

# Job 1:

Used FileInputFormat with a customized RecordReader as the records are in a pattern of 14 lines seperated by \n
Mapper:

KEY-IN:<LONGWRITABLE> It is LongWritable pointing to the offset of the file to be read and pageranked.
VALUE-IN :<TEXT> It points to each record in the file as read by custom Record reader.

KEY-OUT :<TEXT> It is the article name from the file that needs to be page ranked from the whole corpus
VALUE-OUT :<TEXT> It has the Revision ID of the article and Ouu=tlinks from the article all seperated by " "

Reducer:

The KEY-IN and VALUE-IN to Reducer are the outputs from above Mapper.

KEY-IN : <TEXT>
VALUE-IN : <TEXT>
  
KEY-OUT : <TEXT>It is the distinct Article name that has maximum Revision ID and we are taking those outlinks ignoring all old versions of the article.
VALUE-OUT :<TEXT> The maximum Revision ID for the article and all the corresponding outlinks again separated by \n. 
  
  # Job 2:
  
  This runs based on the input args[2] -- no.of iterations specified by user
  
   Here, a FileInputFormat with an existing LineRecordReader is specified as the data in file is in KEY-VALUE format.
   
   Mapper: 
   KEY-IN :<LONGWRITABLE> Points to the offset of our file and each line is fed by Record reader to map method.
  VALUE-IN: <TEXT> It is the the KEY-VALUE pair file outputted by 1st job.
  
  KEY-OUT : <TEXT> Article names. Here we have every outlink as a KEY from all articles along with the original line 
  VALUE_OUT : <TEXT>  It has the revision ID modified based on the contribution it got PR(v)/L(v) at the start along with Article name from which it got the contribution. The original line is also retained where revision ID is appended with : at the start.
  
  Reducer :
  
  The output from above mapper are inputs here.
  KEY-IN : <TEXT>
  VALUE-IN : <TEXT>
  KEY-OUT : <TEXT> It has the unique article names 
  VALUE-OUT : <TEXT> It as the Revision ID replaced by page rank score from 1st iteration appended with all outlinks again seperated by " ". We still retain the :at the start of page rank score.
  
  # Job 3:
  
  This is used to obtain the output in the format specified in coursework. There is only mapper here.
  
   Also here, a FileInputFormat with an existing LineRecordReader is specified as the data in file is in KEY-VALUE format.
  Mapper: 
  KEY-IN : <LONGWRITABLE> Points to the offset of our file and each line is fed by Record reader to map method.
  VALUE-IN :<TEXT> It is the the KEY-VALUE pair file outputted by 2nd job.
  
  KEY-OUT : <TEXT> Unique ArticleName
  VALUE-OUT :<TEXT> final pagerank score obtained after n number of iterations specified.

