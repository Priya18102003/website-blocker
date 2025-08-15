import os

# Determine the host file path based on the operating system
host_path = "C:\\Windows\\System32\\drivers\\etc\\hosts" if os.name == "nt" else "/etc/hosts"
redirect_ip = "127.0.0.1"
blocked_websites = ["www.facebook.com"]
password = "priya2003"

def block_websites():
    try:
        with open(host_path, "r+") as file:
            content = file.read()
            for site in blocked_websites:
                if site not in content:
                    file.write(f"{redirect_ip} {site}\n")
                    print(f"Successfully blocked: {site}")
                else:
                    print(f"{site} is already blocked.")
    except PermissionError:
        print("Permission denied: Unable to modify the hosts file. Please run the script with administrative privileges.")
    except Exception as e:
        print(f"An error occurred while blocking websites: {e}")

def unblock_websites():
    entered_password = input("Enter password to unblock websites: ")
    if entered_password == password:
        try:
            with open(host_path, "r") as file:
                lines = file.readlines()
            with open(host_path, "w") as file:
                for line in lines:
                    if not any(site in line for site in blocked_websites):
                        file.write(line)
            print("Successfully unblocked specified websites.")
        except PermissionError:
            print("Permission denied: Unable to modify the hosts file. Please run the script with administrative privileges.")
        except Exception as e:
            print(f"An error occurred while unblocking websites: {e}")
    else:
        print("Incorrect password! Access denied.")

def main():
    while True:
        choice = input("Enter 'block' to block websites, 'unblock' to unblock websites, or 'exit' to quit: ").strip().lower()
        print(f"users input:{choice}")
        if choice == "block":
            block_websites()
        elif choice == "unblock":
            unblock_websites()
        elif choice == "exit":
            print("Exiting the program.")
            break
        else:
            print("Invalid input. Please enter 'block', 'unblock', or 'exit'.")

if __name__ == "__main__":
    main()
