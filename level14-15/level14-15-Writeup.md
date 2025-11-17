# **Bandit Level 14 → Level 15**

## **Level Goal**

The password for the next level can be retrieved by submitting the current level’s password to a service listening on **localhost port 30000**.
Your task is to connect to this local TCP service, provide the correct credential, and capture the returned password for Level 15.

---

## **Concepts Covered**

* Localhost & loopback networking
* TCP ports and socket communication
* Using Netcat (`nc`) as a network client
* Secure handling of secrets
* Basic challenge–response authentication flow

---

## **Key Tools**

### **Netcat (`nc`)**

A lightweight utility used to:

* Open TCP/UDP connections
* Send and receive raw data
* Test services and ports

In this level, Netcat functions as the client for communicating with the service on port **30000**.

### **File Access (`cat`)**

Used to read the current level’s password from the protected path:

```
/etc/bandit_pass/bandit14
```

---

## **Approach Summary**

To solve this level, you will:

1. Connect to the TCP service on localhost to verify it’s reachable.
2. Submit the current password to the service.
3. Capture the returned password for Level 15.

---

## **Step-by-Step Solution**

### **1. Validate the Service**

Check that port **30000** is listening and responding:

```bash
nc -v localhost 30000
```

You should see a message indicating the connection succeeded and that the service is awaiting input.

---

### **2. Send the Current Password**

Pipe the current level’s password directly into the network connection:

```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

If the submitted password is correct, the service will return the password for Level 15.

---

## **Explanation**

This level demonstrates how network services accept input over TCP and return responses based on the data provided.
Even though everything runs locally, the interaction follows the same model as remote client–server communication:

* Establish TCP connection
* Send authentication data
* Receive response

Netcat is used here because it exposes this interaction in a simple, transparent way—ideal for learning basic networking mechanics.

---

## **Commands Used**

```
nc -v localhost 30000
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

---

