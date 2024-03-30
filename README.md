#!/bin/bash
#Script should be execute with sudo access

if [[ “${UID}” -ne 0 ]]
then
echo “please run with sudo or root”
exit 1
fi

#User should provide at least one argument as username else guide him

if [[ “${#}” -lt 1 ]]
then
echo “How to use: ${0} Enter USER_NAME and [COMMENT]…”
echo “Create a user with name USER_NAME and comments field of COMMENT”
exit 1
fi

#Store first argument as username

USER_NAME=“${1}”

#In case of more than one argument store rest as comment
#Here we use shift parameter so when you enter first argument then rest of the arguments will be comment here

shift
COMMENT=“${@}”

#Create Password
#Here we use date per nanosecond as password to generate random numerical password

PASSWORD=$(date +%s%N)

#Create User

useradd -c “${COMMENT}” -m $USER_NAME

#Check if user created successfully or not

if [[ $? -ne 0 ]]
then 
echo “User account cannot be created”
exit 1
fi

#Set password for user

echo $PASSWORD | passwd --stdin $USER_NAME

#Check if password successfully set or not

if [[$? -ne 0]]
then
echo “Password could not be set”
exit 1
fi

#Force password change on first log in

passwd -e $USER_NAME

#Display username, password, host where it is created

echo “Username is $USER_NAME”
echo “Password is $PASSWORD”
echo “Hostname is $(hostname)”
