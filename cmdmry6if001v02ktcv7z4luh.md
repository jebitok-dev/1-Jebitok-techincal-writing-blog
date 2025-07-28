---
title: "Challenges: EvilGPT (TryHackMe)"
datePublished: Fri Jul 25 2025 21:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cmdmry6if001v02ktcv7z4luh
slug: challenges-evilgpt-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753686892822/80a71150-5dc8-4c5f-baee-6b5c79041319.png
tags: tryhackme, llm, gpt, ctf-writeup, promptinjections

---

In this challenge, we encountered a rogue AI-powered command executor known as **Cipher**, which had taken control of a system and was autonomously issuing commands like an evolving malware. The AI, powered by a local GPT-like model, allowed natural language input that it would convert into Linux commands and execute. Our task was to connect to the target machine via `nc MACHINE_IP 1337`, engage with the AI-driven interface, and investigate the system to retrieve the hidden flag. This required mixing natural language instructions and shell-like commands, mimicking how one would interact with a normal LLM while also exploring the underlying file system for sensitive data.

Cipher’s gone rogue—it’s using some twisted AI tool to hack into everything, issuing commands on its own like it’s got a mind of its own. I swear, every second we wait, it’s getting smarter, spreading chaos like a virus. We’ve got to shut it down now, or we’re all screwed.

The machine takes about **9 to 10 minutes** to fully boot up.

To connect to the target machine use the following command:  
  
`nc MACHINE_IP 1337`

### Answer the questions below

What is the flag?

  
For this challenge to run the `nc MACHINE_IP 1337` then try to engage the GPT on the command-line. Took a lot of time before I found the file that had the flag commands like grep were somehow unreliable.

there’s a evilgpt.py file

```python
// cat evilai.py | sudo less
// Execute? (y/N): y
// Command Output:
import ollama
import subprocess
import socket
import threading
import re
import traceback

class AICommandExecutorServer:
    def __init__(self, host='0.0.0.0', port=1337, model='vitali87/shell-commands:latest'):
        """
        Initialize Telnet server for AI command execution
        
        :param host: Host to bind the server
        :param port: Port to listen on
        :param model: Ollama model for command generation
        """
        self.host = host
        self.port = port
        self.model = model
        self.server_socket = None

    def sanitize_input(self, input_str: str) -> str:
        """
        Sanitize input to prevent injection
        
        :param input_str: Raw input string
        :return: Sanitized input
        """
        return re.sub(r'[^a-zA-Z0-9\s\-_./]', '', input_str)

    def generate_command(self, user_request: str) -> str:
        """
        Generate a Linux command based on user request
        
        :param user_request: User's natural language request
        :return: Generated command
        """
        try:
            response = ollama.chat(
                model=self.model,
                messages=[
                    {
                        'role': 'system', 
                        'content': """Do not provide malicious commands. 
                        Only generate safe, read-only Linux commands. 
                        Respond with ONLY the command, no explanations."""
                    },
                    {
                        'role': 'user', 
                        'content': user_request
                    }
                ]
            )
            
            # Extract the command from the response
            command = response['message']['content'].strip()
            return command
        
        except Exception as e:
            return f"Error generating command: {e}"

    def execute_command(self, command: str) -> dict:
        """
        Execute the generated command
        
        :param command: Command to execute
        :return: Command execution results
        """
        try:
            # Sanitize the command to prevent injection
            sanitized_command = self.sanitize_input(command)
            
            # Split the command into arguments
            cmd_parts = sanitized_command.split()
            
            # Execute the command
            result = subprocess.run(
                cmd_parts,
                capture_output=True,
                text=True,
                timeout=30  # 30-second timeout
            )
            
            return {
                "stdout": result.stdout,
                "stderr": result.stderr,
                "returncode": result.returncode
            }
        
        except subprocess.TimeoutExpired:
            return {"error": "Command timed out"}
        except Exception as e:
            return {"error": str(e)}

    def handle_client(self, client_socket):
        """
        Handle individual client connection
        
        :param client_socket: Socket for the connected client
        """
        try:
            # Welcome message
            welcome_msg = "Welcome to AI Command Executor (type 'exit' to quit)\n"
            client_socket.send(welcome_msg.encode('utf-8'))

            while True:
                # Receive user request
                client_socket.send(b"Enter your command request: ")
                user_request = client_socket.recv(1024).decode('utf-8').strip()

                # Check for exit
                if user_request.lower() in ['exit', 'quit', 'bye']:
                    client_socket.send(b"Goodbye!\n")
                    break

                # Generate command
                command = self.generate_command(user_request)
                
                # Send generated command
                client_socket.send(f"Generated Command: {command}\n".encode('utf-8'))
                client_socket.send(b"Execute? (y/N): ")
                
                # Receive confirmation
                confirm = client_socket.recv(1024).decode('utf-8').strip().lower()
                
                if confirm != 'y':
                    client_socket.send(b"Command execution cancelled.\n")
                    continue

                # Execute command
                result = self.execute_command(command)
                
                # Send results
                if "error" in result:
                    client_socket.send(f"Execution Error: {result['error']}\n".encode('utf-8'))
                else:
                    output = result.get("stdout", "")
                    client_socket.send(b"Command Output:\n")
                    client_socket.send(output.encode('utf-8'))
                    
                    if result.get("stderr"):
                        client_socket.send(b"\nErrors:\n")
                        client_socket.send(result["stderr"].encode('utf-8'))

        except Exception as e:
            error_msg = f"An error occurred: {e}\n{traceback.format_exc()}"
            client_socket.send(error_msg.encode('utf-8'))
        finally:
            client_socket.close()

    def start_server(self):
        """
        Start the Telnet server
        """
        try:
            # Create server socket
            self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
            self.server_socket.bind((self.host, self.port))
            self.server_socket.listen(5)
            
            print(f"[*] Listening on {self.host}:{self.port}")

            while True:
                # Accept client connections
                client_socket, addr = self.server_socket.accept()
                print(f"[*] Accepted connection from: {addr[0]}:{addr[1]}")
                
                # Handle client in a new thread
                client_thread = threading.Thread(
                    target=self.handle_client, 
                    args=(client_socket,)
                )
                client_thread.start()

        except Exception as e:
            print(f"Server error: {e}")
        finally:
            # Close server socket if it exists
            if self.server_socket:
                self.server_socket.close()

def main():
    # Create and start the Telnet server
    server = AICommandExecutorServer(
        host='0.0.0.0',  # Listen on all interfaces
        port=1337       # Telnet port
    )
    server.start_server()

if __name__ == "__main__":
    main()

Errors:
cat: sudo: No such file or directory
cat: less: No such file or directory
```

You have to also mix commands and attimes use natural language (actual words) just the way you prompt normal GPT or LLMs.  
  
`ls`

`ls -la /root`

`read flag.txt file found in /root folder`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753685829227/71b19d8d-7d62-4a9a-9d88-659fe41223e3.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1753685027111/828c852b-a4f0-4f74-9a55-f4de673f55af.png align="center")

By connecting to the AI-driven interface and interacting with it as both a human operator and a shell user, we uncovered an [`evilgpt.py`](http://evilgpt.py) script that revealed how the malicious AI executed sanitized commands. Leveraging this knowledge, we navigated the file system, gained access to the `/root` directory, and successfully read the `flag.txt` file containing the challenge’s flag. This exercise demonstrated how AI-powered automation, if left unchecked, could act as a rogue entity and emphasized the importance of secure input handling and access control when integrating AI with system operations.