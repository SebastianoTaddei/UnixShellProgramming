# Unix Shell Programming: Hands-on Practice

During this session, we will develop a shell script to automate the process of organizing files in a directory. The script will create a new directory for each file type and move the files to the corresponding directory.

## Problem Statement

We will break down the problem into smaller tasks. There are probably more tasks than we have time to cover, but you can always continue working on the script after the session.

- Create a script that accepts a directory as an argument.
- List all files in the directory.
- Create a new directory for each file type.
- Move the files to the corresponding directory.
- Print a summary of the files moved.
- Print statistics about the files moved.
- Add proper error handling.
- Add configuration options.

## Solution

### Preliminary steps

First things first, let's create a new empty directory named `organize_files` in your home directory.

```bash
mkdir ~/organize_files
```

Then, let's navigate to the new directory:

```bash
cd ~/organize_files
```

Once inside, create a new empty script file named `organize_files.sh`.

```bash
touch organize_files.sh
```

We will also make it executable using:

```bash
chmod u+x organize_files.sh
```

Then, we need to obtain the example files stored in this repository. To do so, follow these steps:

- Download the repository to you home directory:
  
  ```bash
  curl -L https://github.com/SebastianoTaddei/UnixShellProgramming/archive/refs/heads/main.zip -o ~/unix_shell_programming.zip
  ```

  This will download the repository as a zip file named `unix_shell_programming.zip` to your home directory.
- Unzip the downloaded file:
  
  ```bash
  unzip ~/unix_shell_programming.zip -d ~
  ```

  This will extract the repository to your home directory and use the default folder structure found in the `.zip` file.
- Copy the `data` folder to the `organize_files` directory:
  
  ```bash
  cp -r ~/UnixShellProgramming-main/data .
  ```

  This will copy the `data` folder to the current directory, which should be the `organize_files` directory.
- Verify that the `data` folder has been copied:
  
  ```bash
  ls .
  ```

  This will list the contents of the `organize_files` directory, and you should see the `data` folder.
- Verify the contents of the `data` folder:
  
  ```bash
  ls data
  ```

  This will list the contents of the `data` folder, and you should see the example files.
- Remove the downloaded zip file:
  
  ```bash
  rm ~/unix_shell_programming.zip
  ```

  This will remove the downloaded zip file from your home directory.
- Remove the extracted repository:
  
  ```bash
  rm -r ~/UnixShellProgramming-main
  ```

  This will remove the extracted repository from your home directory.

### Step 1: Create a script that accepts a directory as an argument

Open the script in your favorite text editor and add the following code:

```bash
#!/bin/bash

echo "Organizing files in directory: $1"
```

Verify that the script is working by running it with a directory as an argument:

```bash
./organize_files.sh /path/to/directory
```

If everything is working as expected, you should see the following output:

```bash
Organizing files in directory: /path/to/directory
```

Before moving to the next step, we can add some error handling. Let's modify the script to check that the user passed one argument, and that the argument is a valid directory:

```bash
#!/bin/bash

if [ "$#" -ne 1 ]; then
    echo "Usage: $0 <directory>"
    exit 1
fi

if [ ! -d "$1" ]; then
    echo "Error: $1 is not a directory"
    exit 1
fi

echo "Organizing files in directory: $1"
```

If we now run the script without any arguments, we should see the following output:

```bash
Usage: ./organize_files.sh <directory>
```

Or if we pass an invalid directory as an argument:

```bash
Error: /invalid/directory is not a directory
```

If we pass a valid directory as an argument, we should see the expected output:

```bash
Organizing files in directory: /path/to/directory
```

We are now ready to move to the next step.
