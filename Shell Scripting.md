# Shell Scripting

* Print something no the Terminal screen:

	```bash
	echo "hello there!"
	```
	
## Redirecting Output

* Write(append) to a file:

	```bash
	echo "hello there" >> file.txt
	```
* Override file contents:

	```bash
	echo "new text" > file.txt
	```
	
## Examples:

* count the words:

	```bash
	# redirecting the input from file.txt to wc(word count utility)
	wc -l < file.txt 
	
	# pipeing the output of the cat command to another command:
	cat file.txt | wc -l
	```
	
* some boolean logic:

	```bash
	# check if the execution went without any problems.
	(cat file.txt | wc -l) && echo "Done!" # if there is a problem with pipeing, the echo part won't execute.
	
	# or condition:
	(cat file.txt | wc -l) || echo "Done!" # would print Done! any way.
	```

## Variables

* Some examples:
	```bash
	echo $HOME  # bash constant
	```
	
	```bash
	set a variable:
	my_var="this is my variable" # NO spaces around equal sign!!!
	echo my_var
	```

	```bash
	number=4
	echo "this is my number: $number"
	echo "this is my ${number}th number"
	```

	```bash
	echo "there are `cat file.txt | wl -l` words in the file"
	#or:
	num_lines=`cat file.txt | wl -l`
	echo "there are $num_lines words in the file"
	```
	
## Script example:

* Find the location of the bash binary:
	```bash
	which bash
	```

* So, put in the script:

	```bash
	#!/bin/bash
	```

* Execute the script:

	```bash
	./myScript.sh
	```
	
	or: 
	
	```bash
	myScript.sh
	```
	
* A script example:

	```bash
	#!/bin/bash
	
	echo "hello world!"
	
	exit 0 # no errors have been encountered. exit $?    -> will exit with the status of the last run command.
	```
	
* Turn the execute bit on the file:
	
	```bash	
	chmod +x myScript.sh
	```
	
	
	
## Arguments and Conditionals:

* Arguments:
	
	`$#` -- number of args that our script was run with
	
	`$0` -- the filename of our script
	
	`$1..$n` -- script arguments
	
* Conditionals:

	```bash
	if [ condition ]; then
		#statements
		else #is the default case, and is optional
	fi 
	```

	```bash
	for (( i = 0; i < 10; i++ )); do
		#statements
	done
	```

	```bash
	for arg in $@; do
		echo $arg 		# will print all passed arguments.
	done
	```
	
## Functions:

```bash
function foo(){ 		# parameters are not named. they are positional, and startig with $1
	#func body here.
}
```

* To call the funcion:
	
	```bash
	foo argumentsYou WantToPass
	```

* Read something:

	```bash
	read variableToBeSet
	```
	
	
	
	
	
	
	
	
	
