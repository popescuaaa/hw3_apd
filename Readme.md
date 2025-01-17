# Homework 3 APD Popescu Andrei Gabriel 333CA 

![Image of Yaktocat](https://octodex.github.com/images/adventure-cat.png)

## The homework consist of a process of read and write applied to an image, process which is intermediate by a a multitude of distributed matrix operations for filtering with a const specificied 3 * 3 2d array called 'filter'

> - During the homewrok the main issue was the actual filtering process and also the fact that the processes can't open simultaneously the same file as the algorithm will be seqvential and the scalability for one process will drop.

> - For the first problem with the matrix filtering the solution was a specific order of the operations beacause of the fact that floats in C programming language has a unpredictied behaviour for diffrent machines and diffrenet enviromnets: in this case a Windows subsystem and a Ubuntu 18.04 native machine.

> - For the scalability part I choosed to share to all processes the input matrix as they will need it and of course they will need to know the specific place where to process and then send thre result to the
master. 

> How is this helfull? 
> - As all the slaves know the initial image the actions that they will perform on it will be the most cpu consumming ones not the sending and receving of small parts. That's the reason why many people consider the spliting process in distrubuted algorithms good until a specific point.

> - The whole system was a very basic implementation of the Apache Spark system architecture as I worked with it a little bit and it seems very efficient: Master -> slaves and a communtion between them.

## Make rules
> - make build: creates from hw-3-apd the main entry of the archive the executable called tema3 
> - make clean: just erase the executable

## Usage and how it works

> The usage of the program:  
> - mpirun -np P ./tema3 input_image(.pgm/.pnm) output_image(.pgm/.pnm) [filters list !!! at least one]

> How it works:
- As I mentioned the main process called Master will read the image from the input file, then it will create a copy of it, basically read the same image twice and it will send the copy to all the salves, then the main process will filter its own part from the image (this is not from the Apache philosophy but it helps with the speed), then will receive from the all the salves the computer images parts and will repeat the process for all other filters

> Similarly, the slave process will receive the image from the input and then will process its own part by distrbuted calculation of the limits, then will send it back to master and repeat it for every filter

## Scalability

Considering the fact that thre network is obviously imperfect and has a consitent delay over data delivery I can say that the programm performs pretty good.

The scalability was tested with a python script that call the process for all filter and all numbers of processes, in my case
there is a small list of [1,2,3,4] because more than 4 processes will be performed in switch mode by the OS and it has a bad delay
and finally the script analysis with a liniar time plot the scale factor almost 0.8 in my case. Here are a couple of tests on scalability:

# 1 process on average

> - real    0m2.148s
> - user    0m1.734s
> - sys     0m0.250s

# 2 processes on average
> - real    0m1.870s
> - user    0m2.547s
> - sys     0m0.609s

# 3 processes on average

> - real    0m1.664s
> - user    0m2.875s
> - sys     0m0.781s

# 4 processes on average
> - real    0m1.596s
> - user    0m3.328s
> - sys     0m0.859s





