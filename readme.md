# ipAnon
This is a fairly basic PowerShell script to anonymize IP addresses with persistent key/value (original/randomized) pairs. It also allows you to set the first octet of your own internal IP range for easier identification.

## Functions
The script takes the following mandatory parameters:
* -keyFile "[location of key file]"
* -inputFile "[location of input file]"
* -privFrom "[IP range start]"
* -privTo "[IP range stop]"


The script will see if a key file (key.csv) exists. If this exists, it will load the key-value pairs into the $hash hashtable. This scripts also takes an input file (ip.txt) and reads the IP addresses located within. Next the script will loop through these IP addresses and check if a $key with this "name" already exist in the $hash hashtable, if they do the $ip will be added to the output with the existing $value. If the $ip does not exist as a key it will be randomized and added to the $hash.

The randomIP function will check if an $ip is in the given "private" ip range, if this is the case it will get a random IP starting with 1. if the $ip is not in the private range it will get an random IP with a starting number between 2-254.

## Constraints
* The input file consists of a list of (one per line) ip addresses (example in [ip.txt](ip.txt))
* Key file contains a key/value pair in "key","value" format (example in [key.csv](key.csv))


## Example
Example with an internal ip range from 10.0.0.0 - 10.0.50.255 (10.0.0.0/12)
```powershell
.\ipAnon.ps1 -keyFile .\key.csv -inputFile .\ip.txt -privFrom "10.0.0.0" -privTo "10.0.63.255"
```

Example with no internal IP range distinction
```powershell
.\ipAnon.ps1 -keyFile .\key.csv -inputFile .\ip.txt -privFrom "0.0.0.0" -privTo "0.0.0.0"
```

## Docker
If you want to use this script in Linux hosts that does not have [PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-linux?view=powershell-7.2) installed you can use the provided docker container

### Docker build
To build the docker image change directories to the ipAnon folder and execute the following command
```bash
docker build -t ipAnon:latest .
```

### Running script
Run the container
```bash
docker run -it --rm -v ${PWD}:/code --name ipAnon ipAnon:latest
```

Once inside the container you can run it just like on a Windows system (as seen in [example](#example))