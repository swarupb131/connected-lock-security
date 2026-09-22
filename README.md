# connected-lock-security
Security analysis of a network-controlled smart lock with vulnerability demonstrations and security mitigations.
# Connected Lock Security Analysis

## Project Overview

This mini project demonstrates the security analysis of a software-based network-controlled smart lock.

The project follows a simple security workflow:

**Build → Attack → Harden → Compare**

A vulnerable lock server is first created and attacked using three security weaknesses. The system is then hardened using HTTPS/TLS, challenge-response authentication, replay protection, authorization checks, and signed firmware verification.

## Objectives

The objectives of this project are:

* Understand security issues in connected IoT devices.
* Demonstrate common vulnerabilities in a smart lock.
* Implement attacks against the vulnerable design.
* Apply security mitigations.
* Compare the vulnerable and hardened systems.
* Demonstrate secure firmware verification.

## System Architecture

```text
Client Application
       |
       | HTTP / HTTPS
       |
       v
   Lock Server
       |
       v
  Smart Lock
```

The client sends requests to the lock server. The server authenticates the user and controls the simulated lock state.

## Vulnerabilities Demonstrated

### 1. Plaintext Credentials

The vulnerable system uses HTTP.

Authentication information can therefore be exposed if network traffic is intercepted.

### 2. Replay Attack

The vulnerable system uses a static unlock token:

```text
UNLOCK-12345
```

An attacker who captures the token can replay the same request and unlock the lock again.

### 3. Missing Authorization Check

The vulnerable server checks whether the unlock token is valid but does not check the user's role.

Therefore, a guest user can unlock the lock if they obtain the token.

## Security Mitigations

### HTTPS / TLS

TLS is used in the hardened version to protect communication between the client and server.

### Challenge-Response Authentication

The server generates a random nonce.

The client uses the nonce and its secret key to generate an HMAC response.

The server verifies the HMAC before unlocking the lock.

### Replay Protection

The nonce is removed after successful authentication.

Therefore, an old authentication request cannot be reused.

### Authorization

Only users with the `owner` role are allowed to unlock the hardened lock.

### Firmware Verification

Firmware is verified using a cryptographic signature.

Valid firmware is accepted while modified or invalid firmware is rejected.

## Before vs After

| Security Feature      | Vulnerable System | Hardened System           |
| --------------------- | ----------------- | ------------------------- |
| Communication         | HTTP              | HTTPS/TLS                 |
| Authentication        | Static token      | Challenge-response + HMAC |
| Replay Protection     | No                | Yes                       |
| Authorization         | Missing           | Role-based                |
| Firmware Verification | No                | Yes                       |

## Project Demonstration

The program demonstrates:

1. Normal login and unlock.
2. Plaintext credential vulnerability.
3. Replay attack.
4. Missing authorization vulnerability.
5. TLS-protected communication.
6. Challenge-response authentication.
7. Replay attack rejection.
8. Unauthorized guest rejection.
9. Valid firmware acceptance.
10. Modified firmware rejection.

## Technologies Used

* Python
* Flask
* Requests
* HMAC
* SHA-256
* HTTPS / TLS
* OpenSSL
* Google Colab
* GitHub

## How to Run

Install the required packages:

```bash
pip install -r requirements.txt
```

Then run:

```bash
python connected_lock_security.py
```

The program starts a vulnerable server and a hardened server and automatically demonstrates the attacks and security mitigations.

## Security Note

This project is an educational security demonstration. The attacks are performed only against the locally created demonstration server.

## Conclusion

The project demonstrates that a connected IoT lock should not rely on a simple reusable token.

A more secure design combines encrypted communication, challenge-response authentication, replay protection, authorization, and firmware integrity verification.
