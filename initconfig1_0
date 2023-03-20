import sys
from getpass import getpass
from netmiko import ConnectHandler

def main():
    switch_ip = input("Enter the switch IP address: ")
    username = input("Enter your username: ")
    password = getpass("Enter your password: ")
    enable_password = getpass("Enter your enable password: ")

    print("Connecting to the switch...")

    ios_xe_device = {
        "device_type": "cisco_ios",
        "ip": switch_ip,
        "username": username,
        "password": password,
        "secret": enable_password,
    }

    try:
        connection = ConnectHandler(**ios_xe_device)
    except Exception as e:
        print(f"Error connecting to the switch: {e}")
        sys.exit(1)

    connection.enable()

    hostname = input("Enter the desired hostname: ")
    domain_name = input("Enter the domain name: ")

    initial_config_commands = [
        f"hostname {hostname}",
        f"ip domain-name {domain_name}",
        "no ip domain-lookup",
        "spanning-tree mode rapid-pvst",
        "line vty 0 15",
        "transport input ssh",
        "login local",
        "exit",
        "wr",
    ]

    print("Applying initial configuration...")
    output = connection.send_config_set(initial_config_commands)
    print(output)

    print("Disconnecting from the switch.")
    connection.disconnect()

if __name__ == "__main__":
    main()
