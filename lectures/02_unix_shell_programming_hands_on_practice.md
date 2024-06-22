# Unix Shell Programming: Hands-on Practice

During this session, we will develop a shell script to automate the process of organizing files in a directory. The script will create a new directory for each file type and move the files to the corresponding directory.

## Problem Statement

We will break down the problem into smaller tasks. We will cover the basic requirements first and then move on to more advanced features if time permits.

Basic:

- Create a script that accepts a directory as an argument.
- List all files in the directory.
- Organise the files based on their type (e.g., images, documents, videos).

Advanced:

- Print statistics about the files moved.
- Add configuration options.

Many other features can be added to this script, such as error handling, logging, etc. We will focus on the core functionality during this session.

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
- Make a backup of the `data` folder:
  
  ```bash
  cp -r data data_backup
  ```

  This will create a backup of the `data` folder named `data_backup`.

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

### Step 2: List all files in the directory

We are now going to list all files in the directory. Modify the script to include the following code:

```bash
...
files=$(find "$1" -type f)
echo "Found files:"
for file in $files; do
    echo "$file"
done
```

The `find` command is used to search for files in the specified directory. We are using the `-type f` option to only list regular files. The list of files is stored in the `files` variable, and we are iterating over each file to print it. Note that `find` will recursively search for files in subdirectories as well. However, you can see that we are only printing the file with its relative path to the specified directory. Let us modify the script to split the path and print only the file name:

```bash
for file in $files; do
    filename=$(basename "$file")
    echo "$filename"
done
```

If we now run the script with a valid directory as an argument, we should see a list of file names.

### Step 3: Organize the files based on their type

The types of directories we want to create is subjective. For this example we will restrict ourselves to the following file types:

- Images (`.jpg`, `.jpeg`, `.png`, `.gif`)
- Documents (`.pdf`, `.txt`, `.md`)
- Videos (`.mp4`, `.mov`)

Of course you can customise it later to your needs. I suggest you to provide a configuration file to the script to allow the user to specify the file types.

With that in mind, let's modify the script to first divide the file into the three categories. Let us add the following code:

```bash
case "$file" in
    *.jpg|*.jpeg|*.png|*.gif)
        echo "Moving image: $file"
        ;;
    *.pdf|*.txt|*.md)
        echo "Moving document: $file"
        ;;
    *.mp4|*.mov)
        echo "Moving video: $file"
        ;;
    *)
        echo "Unknown file type: $file"
        ;;
esac
```

Let us wrap this code in a function to make it more readable:

```bash
organize_files () {
    case "$1" in
        *.jpg|*.jpeg|*.png|*.gif)
            echo "Moving image: $1"
            ;;
        *.pdf|*.txt|*.md)
            echo "Moving document: $1"
            ;;
        *.mp4|*.mov)
            echo "Moving video: $1"
            ;;
        *)
            echo "Unknown file type: $1"
            ;;
    esac
}
```

Let us break down the code:

- We are using a `case` statement to match the file extension.
- For each match, we are printing a message indicating the file type.
- For unknown file types, we are printing a message indicating that the file type is unknown.

This `case` statement works by matching the file extension using the `*.extension` pattern. For example, `*.jpg` matches all files with the `.jpg` extension (`*` in this context is a wildcard that means "match anything"). The `|` character is used to separate multiple patterns that should be matched. The `*)` pattern is a wildcard that matches any file that does not match the previous patterns.

Now, we just need to call this function for each file in the directory. Simply add the function at the top of the script, and a loop at the end:

```bash
#!/bin/bash

# Organize files based on type
organize_files () {
    ...
}

# Previous code
...

# Move files based on type
for file in $files; do
    filename=$(basename "$file")
    organize_files "$filename"
done
```

Run the script with a valid directory as an argument, and you should see correct messages for each file type. With that working, all we need to do is create the directories and move the files.

First, let's create the directories for each file type. Modify the `organize_files` function to include the following code:

```bash
organize_files () {
    case "$1" in
        *.jpg|*.jpeg|*.png|*.gif)
            echo "Moving image: $1"
            mkdir -p images
            ;;
        *.pdf|*.txt|*.md)
            echo "Moving document: $1"
            mkdir -p documents
            ;;
        *.mp4|*.mov)
            echo "Moving video: $1"
            mkdir -p videos
            ;;
        *)
            echo "Unknown file type: $1"
            ;;
    esac
}
```

The `mkdir -p` command is used to create the directories. The `-p` option ensures that the command does not fail if the directories already exist as well as creating any necessary parent directories. Now, we just need to move the files to the corresponding directories. Modify the `organize_files` function to include the following code:

```bash
organize_files () {
    case "$1" in
        *.jpg|*.jpeg|*.png|*.gif)
            echo "Moving image: $1"
            mkdir -p images
            mv "$1" images/
            ;;
        *.pdf|*.txt|*.md)
            echo "Moving document: $1"
            mkdir -p documents
            mv "$1" documents/
            ;;
        *.mp4|*.mov)
            echo "Moving video: $1"
            mkdir -p videos
            mv "$1" videos/
            ;;
        *)
            echo "Unknown file type: $1"
            ;;
    esac
}
```

The `mv` command is used to move files. The first argument is the file to move, and the second argument is the destination directory. This however will not work as expected, as we are moving the files from the current directory. We need to specify the full path to the file. Modify the `organize_files` function to take as input the full path to the file:

```bash
organize_files () {
    local file="$1"
    local filename=$(basename "$file")
    case "$filename" in
        *.jpg|*.jpeg|*.png|*.gif)
            echo "Moving image: $filename"
            mkdir -p images
            mv "$file" images/
            ;;
        *.pdf|*.txt|*.md)
            echo "Moving document: $filename"
            mkdir -p documents
            mv "$file" documents/
            ;;
        *.mp4|*.mov)
            echo "Moving video: $filename"
            mkdir -p videos
            mv "$file" videos/
            ;;
        *)
            echo "Unknown file type: $filename"
            ;;
    esac
}
```

Running the script with a valid directory as an argument should now move the files to the corresponding directories. One last thing before moving on to the next point: take in a second argument to specify the directory where to move the files. Modify the script to include the following code:

```bash
...

organize_files () {
    local file="$1"
    local filename=$(basename "$file")
    local destination="$2"
    case "$filename" in
        *.jpg|*.jpeg|*.png|*.gif)
            echo "Moving image: $filename to $destination/images"
            mkdir -p "$destination/images"
            mv "$file" "$destination/images/"
            ;;
        *.pdf|*.txt|*.md)
            echo "Moving document: $filename to $destination/documents"
            mkdir -p "$destination/documents"
            mv "$file" "$destination/documents/"
            ;;
        *.mp4|*.mov)
            echo "Moving video: $filename to $destination/videos"
            mkdir -p "$destination/videos"
            mv "$file" "$destination/videos/"
            ;;
        *)
            echo "Unknown file type: $filename"
            ;;
    esac
}

...

if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <directory> <destination>"
    exit 1
fi

...

# Create the destination directory if it does not exist
if [ ! -d "$2" ]; then
    mkdir -p "$2"
fi

...

for file in $files; do
    organize_files "$file" "$2"
done
```

Running this script with a valid directory and destination directory as arguments should now move the files to the corresponding directories in the destination directory.

Before moving on to the advanced requirements, let us summarise and clean-up the script we have written until now:

```bash
#!/bin/bash

# Organize files based on type
organize_files () {
    local file="$1"
    local filename=$(basename "$file")
    local destination="$2"
    case "$filename" in
        *.jpg|*.jpeg|*.png|*.gif)
            echo "Moving image: $filename to $destination/images"
            mkdir -p "$destination/images"
            mv "$file" "$destination/images/"
            ;;
        *.pdf|*.txt|*.md)
            echo "Moving document: $filename to $destination/documents"
            mkdir -p "$destination/documents"
            mv "$file" "$destination/documents/"
            ;;
        *.mp4|*.mov)
            echo "Moving video: $filename to $destination/videos"
            mkdir -p "$destination/videos"
            mv "$file" "$destination/videos/"
            ;;
        *)
            echo "Unknown file type: $filename"
            ;;
    esac
}

# Check if the correct number of arguments is provided
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <directory> <destination>"
    exit 1
fi

# Verify that the directory exists
if [ ! -d "$1" ]; then
    echo "Error: $1 is not a directory"
    exit 1
fi

# Create the destination directory if it does not exist
if [ ! -d "$2" ]; then
    mkdir -p "$2"
fi

echo -e "\nOrganizing files in directory: $1\n"

# Find all files in the directory
files=$(find "$1" -type f)
echo "Found files:"
for file in $files; do
    filename=$(basename "$file")
    echo "- $filename"
done

# Move files based on type
echo -e "\nOrganizing files:"
for file in $files; do
    organize_files "$file" "$2"
done

echo -e "\nFiles organized successfully.\n"
```

The keen observer will notice that we added the `-e` option to the `echo` command to enable interpretation of backslash escapes. This allows us to use the `\n` escape sequence to print newlines.

NOTE: The `-e` option is not supported by all versions of `echo`. If you encounter issues, you can use `printf` instead.

### Step 4: Print statistics about the files moved

Now that we have covered the basic requirements, let us move on to the advanced features. We will now print statistics about the files moved. For example, let us measure the file size of each file moved per category. Modify the `organize_files` function to include the following code:

```bash
organize_files () {
    local file="$1"
    local filename=$(basename "$file")
    local destination="$2"
    local size=$(du -h "$file" | cut -f 1)
    case "$filename" in
        *.jpg|*.jpeg|*.png|*.gif)
            echo "Moving image: $filename ($size) to $destination/images"
            mkdir -p "$destination/images"
            mv "$file" "$destination/images/"
            ;;
        *.pdf|*.txt|*.md)
            echo "Moving document: $filename ($size) to $destination/documents"
            mkdir -p "$destination/documents"
            mv "$file" "$destination/documents/"
            ;;
        *.mp4|*.mov)
            echo "Moving video: $filename ($size) to $destination/videos"
            mkdir -p "$destination/videos"
            mv "$file" "$destination/videos/"
            ;;
        *)
            echo "Unknown file type: $filename"
            ;;
    esac
}
```

We have added the `du` command to get the disk usage of the file. The `-h` option is used to print the output in human-readable format. We then pipe `|` the output of the `du` command to the `cut` command. The `cut` command prints selected parts of lines from its input. The `-f 1` option is used to select the first field of the output, which is the file size. This will give us the file size in human-readable format. We can now print the file size when moving the file.

You can implement much more advanced statistics if you want, like keeping track of the total size of each category, the number of files moved, etc. I leave this as an exercise to the reader.

### Step 5: Add configuration options

Finally, let us add configuration options to the script. We will allow the user to specify the categories and the corresponding file extensions. Several ways to implement this exist, we will focus on a simple one using `bash` arrays and a second `bash` script to provide the configuration. Let us start by creating a new script named `config.sh`:

```bash
# Do not run this script, it is meant to be sourced by the main script

# Define the categories and file extensions
categories=("images" "documents" "videos")
file_extensions=("jpg jpeg png gif" "pdf txt md" "mp4 mov")
```

What we are doing here is defining two arrays, `categories` and `file_extensions`. The `categories` array contains the names of the categories, and the `file_extensions` array contains the file extensions for each category. The order of the elements in the arrays should match. For example, the first element in the `categories` array corresponds to the first element in the `file_extensions` array.

NOTE: We did not include `#!/bin/bash` at the beginning of the script because we do not want to execute it. We will `source` this script in the main script to load the configuration.

We now need to modify the main script to load the configuration. Add the following code at the beginning of the script:

```bash
#!/bin/bash

# Load the configuration
source config.sh

...
```

The `source` command is used to load the configuration script. This will make the variables defined in the `config.sh` script available in the main script. It effectively copies the content of the `config.sh` script into the main script. We can now use the `categories` and `file_extensions` arrays in the main script. Let us modify the `organize_files` function to use the configuration:

```bash
organize_files () {
    local file="$1"
    local filename=$(basename "$file")
    local destination="$2"
    local size=$(du -h "$file" | cut -f 1)
    for i in "${!categories[@]}"; do
        for ext in ${file_extensions[$i]}; do
            if [[ "$filename" == *.$ext ]]; then
                echo "Moving $ext: $filename ($size) to $destination/${categories[$i]}"
                mkdir -p "$destination/${categories[$i]}"
                mv "$file" "$destination/${categories[$i]}/"
                return
            fi
        done
    done
    echo "Unknown file type: $filename"
}
```

Now this is a little complex, so let us break it down:

- We are iterating over the indices of the `categories` array using the `${!categories[@]}` syntax. This allows us to access the index of the array element.
- For each category, we are iterating over the file extensions using the `${file_extensions[$i]}` syntax. This allows us to access the file extensions corresponding to the category.
- We are using the `[[ "$filename" == *.$ext ]]` syntax to match the file extension. The `*.$ext` pattern matches any file with the specified extension.
- If a match is found, we are moving the file to the corresponding category and returning from the function. This ensures that the file is only moved once.
- If no match is found, we are printing a message indicating that the file type is unknown.

Run the script with a valid directory and destination directory as arguments, and you should see the files being moved to the corresponding categories based on the configuration.

NOTE: Allowing the user to specify the categories and file extensions through a configuration file provides flexibility and allows the user to customize the script to their needs. However, it also adds all sorts of problems that may arise from the user providing incorrect or incomplete information. It is up to you to decide how to handle such cases.

The complete scripts should look like this:

`organize_files.sh`:

```bash
#!/bin/bash

# Load the configuration
source config.sh

# Organize files based on type
organize_files () {
    local file="$1"
    local filename=$(basename "$file")
    local destination="$2"
    local size=$(du -h "$file" | cut -f 1)
    for i in "${!categories[@]}"; do
        for ext in ${file_extensions[$i]}; do
            if [[ "$filename" == *.$ext ]]; then
                echo "Moving $ext: $filename ($size) to $destination/${categories[$i]}"
                mkdir -p "$destination/${categories[$i]}"
                mv "$file" "$destination/${categories[$i]}/"
                return
            fi
        done
    done
    echo "Unknown file type: $filename"
}

# Check if the correct number of arguments is provided
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <directory> <destination>"
    exit 1
fi

# Verify that the directory exists
if [ ! -d "$1" ]; then
    echo "Error: $1 is not a directory"
    exit 1
fi

# Create the destination directory if it does not exist
if [ ! -d "$2" ]; then
    mkdir -p "$2"
fi

echo -e "\nOrganizing files in directory: $1\n"

# Find all files in the directory
files=$(find "$1" -type f)
echo "Found files:"
for file in $files; do
    filename=$(basename "$file")
    echo "- $filename"
done

# Move files based on type
echo -e "\nOrganizing files:"
for file in $files; do
    organize_files "$file" "$2"
done

echo -e "\nFiles organized successfully.\n"
```

`config.sh`:

```bash
# Do not run this script, it is meant to be sourced by the main script

# Define the categories and file extensions
categories=("images" "documents" "videos")
file_extensions=("jpg jpeg png gif" "pdf txt md" "mp4 mov")
```

## Conclusion

The script we have developed is a simple example of how you can automate file organization using shell scripting. We have covered the basic requirements and some advanced features such as printing statistics and adding configuration options. You can further enhance the script by adding error handling, logging, and more advanced statistics. The possibilities are endless, and it is up to you to explore and experiment with different features.

I believe that seeing how small and powerful the resulting script is, you can appreciate the power of shell scripting. It is a great tool to automate repetitive os-tasks and improve your productivity. I hope you enjoyed this hands-on practice session, and I encourage you to continue exploring shell scripting and Unix tools.
