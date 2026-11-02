# 2a_Stop_and_Wait_Protocol
## AIM 
To write a python program to perform stop and wait protocol
## ALGORITHM
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM

#server.py — receiver
import socket

HOST = "127.0.0.1"
PORT = 8001

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server:
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind((HOST, PORT))
    server.listen(1)
    print(f"Waiting for sender on {HOST}:{PORT}...")

    connection, address = server.accept()

    with connection:
        print(f"Connected by {address}")

        reader = connection.makefile("r", encoding="utf-8")

        for line in reader:
            line = line.rstrip("\n")

            if line == "END":
                print("Transmission finished.")
                break

            sequence, data = line.split("|", 1)
            print(f"Received frame {sequence}: {data}")

            connection.sendall(f"ACK|{sequence}\n".encode("utf-8"))
#client.py — sender
import socket

HOST = "127.0.0.1"
PORT = 8001

with socket.create_connection((HOST, PORT)) as connection:
    reader = connection.makefile("r", encoding="utf-8")
    sequence = 0

    while True:
        data = input("Enter frame data (or END to finish): ")

        if data.upper() == "END":
            connection.sendall(b"END\n")
            print("Transmission finished.")
            break

        frame = f"{sequence}|{data}\n"
        connection.sendall(frame.encode("utf-8"))
        print(f"Sent frame {sequence}: {data}")

        ack = reader.readline().strip()

        if ack == f"ACK|{sequence}":
            print(f"Received {ack}")
            sequence = 1 - sequence
        else:
            print(f"Unexpected acknowledgement: {ack!r}")
            break
## OUTPUT
<img width="952" height="1013" alt="image" src="https://github.com/user-attachments/assets/8a54e96d-c985-410b-8374-c39bcbb25462" />

<img width="952" height="1018" alt="image" src="https://github.com/user-attachments/assets/2e4a5ef1-7aa1-429a-98ce-b8629a981edc" />


## RESULT
Thus, python program to perform stop and wait protocol was successfully executed.
