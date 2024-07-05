---
title: "Writing a BashScript using Linux Commands and Privileges to Read Employee Users & Group"
datePublished: Thu Jul 04 2024 20:40:22 GMT+0000 (Coordinated Universal Time)
cuid: cly7qdt5h00030amn84fd988e
slug: writing-a-bashscript-using-linux-commands-and-privileges-to-read-employee-users-group
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1720161621095/408881aa-66a8-4e51-bfbe-fca9ccc8f41c.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1720125595610/5b290ee0-adc5-4273-b925-7bfcf698d496.png
tags: linux, devops, bash-script

---

I first came across the [HNG Internship](https://hng.tech/) in 2020, a few months after I had gotten started in tech and I ended up joining their cohort that was running that year for the frontend track. It was a great experience and I learned a lot about projects, collaborations, and the pressure of limited timelines. HNG Internship supports various technical and non-technical tracks, i.e., Backend (C#, Golang, PHP, Python, Node.js), DevOps, Frontend, Mobile, Product Design, Digital Marketing, product testing, data analysis, and product management. If you’re interested in getting experience alongside several people in a gamified manner you [can sign up to HNG](https://hng.tech/) and they also have a [HNG Premium](https://hng.tech/premium) version of the program which includes BootCamp training, one-on-one calls with mentors, assistance in job search, and more packages.

In this article I’m going to talk about the DevOps I’m taking up at HNG which requires writing a bash script that allows a user to input a text file with new employees who are developers and they’re to be assigned into groups, user passwords are to be saved into a file that can only be accessed by the owner and the logs of the script are also saved into a file

## Requirements

* the bash script should be able to take up a text file with employees’ names' and be able to add and assign them to Linux groups and users. users will have the same group name as their username and the group name won't be written in the text file e.g. bash create\_user.sh, file\_with\_employee\_names.txt (added to groups & assigned)
    
* the user can have multiple groups and each group is delimited or separated by a comma but the usernames and groups are separated by semicolons
    
* the script should be able to set up home directories with permission and ownership
    
* the script should generate random passwords for all users and the passwords will be securely stored on /var/secure/user\_passwords.txt. this script should also check if the user already exists on this file - return an error message or so.
    
* the script should log all actions to /var/log/user\_management.log (it creates and contains all actions performed by the script)
    
* the script should store the list of all users and their passwords separated by commas on /var/secure/user.password.cvs. The file owner should be the only one to read it
    

We start by creating a folder locally:

```jsx
$ vagrant up
$ mkdir employeebashscript
$ cd employeebashscript
$ touch create_user.sh
$ touch README.md
```

We initialize our bash script with the bash bang which consists of the directory bash shell program to be used for script execution. The hash exclamation mark #! character sequence is referred to as the Shebang. It’s used to set the path of the bash program.

```bash
#!/bin/bash
```

Next, we will write a function and call that will be used to install some packages we might need in case it's not locally installed once the script is executing

```bash
# function to install some packages if they are not installed
install_packages() {
   if command -v apt-get &>/dev/null; then
         sudo apt-get update
         sudo apt-get install -y passwd openssl || echo "Failed to install packages"
    elif command -v yum &>/dev/null; then
        sudo yum install -y passwd openssl || echo "Failed to install packages"
    fi
}

# Install the required packages
install_packages
```

When the script is executed we will pass a text file that has a list of employees so that the bash script will add and assign them into groups. This part will ensure that we execute the script with a file with employees names

```bash
# Check if the input file with employee names is provided 
if [ -z "$1" ]; then
    echo "Usage: $0 file_with_employee_names.txt"
    exit 1
fi

input_file=$1

# Check if the input file exists
if [ ! -f $input_file ]; then
    echo "File $input_file does not exist"
    exit 1
fi
```

While the script is executed we will need a secure folder that will have a `user_passwords.txt` file that will store the passwords generated for users as well as a `user_management.log` that will keep all logs and a `user_passwords.csv`file that will have a list of users and their passwords which will be only accessed by the file owner

```bash
# Creating secure files and directories if they do not exist
sudo mkdir -p /var/secure
sudo touch /var/secure/user_passwords.txt /var/secure/user_passwords.csv
sudo touch /var/log/user_management.log

```

We will now create a while loop that ensures that the script checks all conditions and gets executed:

* it will read through the files to check whether a username already exists before it gets created
    
* it creates users and groups together with additional groups
    
* ensuring the home directory permissions are set correctly
    
* Generating the random password for the user, checking if the user exists in the password file before storing their password
    
* Check the username if it exists in the file with the list of passwords and usernames before adding them
    

```bash
while IFS= read -r line; do 
    username=$(echo $line | cut -d';' -f1)
    groups=$(echo $line | cut -d';' -f2 | tr ',' ' ')

    if id $username &>/dev/null; then
        echo "User $username already exists" | tee -a /var/log/user_management.log
        continue
    fi

    # Create the User and Group
    sudo useradd -m -s /bin/bash $username
    sudo groupadd $username
    sudo usermod -a -G $username $username

    # Adding additional groups
    for group in $groups; do
        sudo groupadd $group
        sudo usermod -a -G $group $username
    done

    echo "User $username has been created and assigned to groups: $groups" | tee -a /var/log/user_management.log

    # Ensure home directory permissions are set correctly
    home_dir="/home/$username"
    sudo chmod 700 $home_dir
    sudo chown $username:$username $home_dir
    echo "Home directory permissions for $username have been set" | sudo tee -a /var/log/user_management.log
    
    # Generate a random password and set it for the user
    password=$(openssl rand -base64 12)
    echo "$username:$password" | sudo chpasswd

    # Check if the user already exists in the Password file before storing the password in a secure file
    if grep -q "^$username" /var/secure/user_passwords.txt; then
        echo "User $username already exists in the password file" | tee -a /var/log/user_management.log
    else
        echo "$username:$password" | sudo tee -a /var/secure/user_passwords.txt
        echo "User $username has been added to the password file" | tee -a /var/log/user_management.log
    fi

    # Check if the user already exists in the Password file
    if grep -q "^$username," /var/secure/user_password.csv; then
        echo "User $username already exists in the password file" | tee -a /var/log/user_management.log
    else
        echo "$username,$password" | sudo tee -a /var/secure/user_passwords.csv
        echo "User $username has been added to the password file" | tee -a /var/log/user_management.log
    fi

done < $input_file
```

Lastly, we have to secure our password files

```bash
# Secure the password files
sudo chown root:root /var/secure/user_passwords.txt /var/secure/user_passwords.csv
sudo chmod 600 /var/secure/user_passwords.txt /var/secure/user_passwords.csv
```

### Complete Script

```bash
#!/bin/bash

# function to install some packages if they are not installed
install_packages() {
   if command -v apt-get &>/dev/null; then
         sudo apt-get update
         sudo apt-get install -y passwd openssl || echo "Failed to install packages"
    elif command -v yum &>/dev/null; then
        sudo yum install -y passwd openssl || echo "Failed to install packages"
    fi
}

# Install the required packages
install_packages

# Check if the input file with employee names is provided 
if [ -z "$1" ]; then
    echo "Usage: $0 file_with_employee_names.txt"
    exit 1
fi

input_file=$1

# Check if the input file exists
if [ ! -f $input_file ]; then
    echo "File $input_file does not exist"
    exit 1
fi

# Creating secure files and directories if they do not exist
sudo mkdir -p /var/secure
sudo touch /var/secure/user_passwords.txt /var/secure/user_passwords.csv
sudo touch /var/log/user_management.log

while IFS= read -r line; do 
    username=$(echo $line | cut -d';' -f1)
    groups=$(echo $line | cut -d';' -f2 | tr ',' ' ')

    if id $username &>/dev/null; then
        echo "User $username already exists" | tee -a /var/log/user_management.log
        continue
    fi

    # Create the User and Group
    sudo useradd -m -s /bin/bash $username
    sudo groupadd $username
    sudo usermod -a -G $username $username

    # Adding additional groups
    for group in $groups; do
        sudo groupadd $group
        sudo usermod -a -G $group $username
    done

    echo "User $username has been created and assigned to groups: $groups" | tee -a /var/log/user_management.log

    # Ensure home directory permissions are set correctly
    home_dir="/home/$username"
    sudo chmod 700 $home_dir
    sudo chown $username:$username $home_dir
    echo "Home directory permissions for $username have been set" | sudo tee -a /var/log/user_management.log
    
    # Generate a random password and set it for the user
    password=$(openssl rand -base64 12)
    echo "$username:$password" | sudo chpasswd

    # Check if the user already exists in the Password file before storing the password in a secure file
    if grep -q "^$username" /var/secure/user_passwords.txt; then
        echo "User $username already exists in the password file" | tee -a /var/log/user_management.log
    else
        echo "$username:$password" | sudo tee -a /var/secure/user_passwords.txt
        echo "User $username has been added to the password file" | tee -a /var/log/user_management.log
    fi

    # Check if the user already exists in the Password file
    if grep -q "^$username," /var/secure/user_password.csv; then
        echo "User $username already exists in the password file" | tee -a /var/log/user_management.log
    else
        echo "$username,$password" | sudo tee -a /var/secure/user_passwords.csv
        echo "User $username has been added to the password file" | tee -a /var/log/user_management.log
    fi

done < $input_file

# Secure the password files
sudo chown root:root /var/secure/user_passwords.txt /var/secure/user_passwords.csv
sudo chmod 600 /var/secure/user_passwords.txt /var/secure/user_passwords.csv

echo "User creation process has been completed successfully"
```

### Making the Script Executable and Executing

```bash
$ chmod +x create_user.sh
```

We will need a sample text file with the employees to test the bash script `file_with_employee_names.txt`

```bash
$ touch file_with_employee_names.txt
$ nano file_with_employee_names.txt
$ cat file_with_employee_names.txt

alice;dev,admin
bob;dev
charlie;admin
dave;dev,qa,admin
eve;qa

```

```bash
$ ./create_user.sh file_with_employee_names.txt
```

Thanks for reading through the article. It has been an interesting experience getting started with DevOps learning how to write BashScripts and understanding how they help ease the deployment process. You can leave a comment regarding the article, and what I can improve on, or we can connect further through [X](https://x.com/SharonJebitok) or [LinkedIn](https://www.linkedin.com/in/sharon-jebitok/)