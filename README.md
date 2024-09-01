# MailTM Python SDK

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

The MailTM Python SDK provides a simple and efficient way to interact with the [Mail.tm API](https://mail.tm), which allows you to create temporary email accounts and manage messages programmatically. This SDK is ideal for automated testing, handling email verifications, and other scenarios where disposable email addresses are useful.

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)
  - [Creating an Account](#creating-an-account)
  - [Fetching Domains](#fetching-domains)
  - [Retrieving Messages](#retrieving-messages)
  - [Deleting Messages](#deleting-messages)
- [Methods](#methods)
- [Contributing](#contributing)
- [License](#license)

## Introduction

MailTM is a temporary email service that provides disposable email addresses. This SDK allows developers to interact with the MailTM API to create temporary email accounts, retrieve and manage messages, and delete accounts when they are no longer needed.

## Installation

To install the MailTM Python SDK, simply use `pip`:

```bash
pip install mailtm-python
```

To install nightly builds, use the following command:

```bash
pip install git+https://github.com/viperadnan-git/mailtm-python-sdk.git --force-reinstall
```

Alternatively, you can clone the repository and install the dependencies manually:

```bash
git clone https://github.com/viperadnan-git/mailtm-python-sdk.git
cd mailtm-python-sdk
pip install -r requirements.txt
```

## Usage

### Creating an Account

To create a new temporary email account, you can use the `create_account` method. Here's an example:

```python
from mailtm import MailTMClient

# Fetch available domains
domains = MailTMClient.get_domains()

# Create a new account using one of the domains
email_address = f"mytempemail@{domains[0].domain}"
password = "supersecretpassword"

client = MailTMClient.create_account(address=email_address, password=password)
print(f"Account created with email: {client.address}")
```

### Fetching Domains

To fetch the list of available domains that can be used for creating temporary email addresses:

```python
domains = MailTMClient.get_domains()
for domain in domains:
    print(f"Domain: {domain.domain}")
```

### Retrieving Messages

After creating an account and authenticating, you can retrieve messages for that account:

```python
# Fetch messages
messages = client.get_messages()
for message in messages:
    print(f"From: {message.from_.address}, Subject: {message.subject}")
```

### Deleting Messages

You can delete a specific message by its ID:

```python
message_id = messages[0].id
client.delete_message(message_id)
print(f"Message {message_id} deleted.")
```

## Methods

The following methods are provided by the `MailTMClient` class:

- **`MailTMClient.get_domains(page: int = 1, proxies: Optional[Dict[str, str]] = None) -> List[Domain]`**: Fetch the available domains for creating an account.
  
  Example:
  
  ```python
  domains = MailTMClient.get_domains(proxies={"http": "http://proxy.example.com:8080"})
  for domain in domains:
      print(f"Domain: {domain.domain}")
  ```

- **`MailTMClient.get_domain_by_id(domain_id: str, proxies: Optional[Dict[str, str]] = None) -> Domain`**: Retrieve details of a specific domain by its ID.
  
  Example:
  
  ```python
  domain = MailTMClient.get_domain_by_id("some-domain-id", proxies={"http": "http://proxy.example.com:8080"})
  print(f"Domain: {domain.domain}")
  ```

- **`MailTMClient.create_account(address: str, password: str, proxies: Optional[Dict[str, str]] = None) -> Account`**: Create a new temporary email account.
  
  Example:
  
  ```python
  account = MailTMClient.create_account("user@domain.com", "password", proxies={"http": "http://proxy.example.com:8080"})
  print(f"Account created with email: {account.address}")
  ```

- **`MailTMClient.get_token(address: str, password: str) -> TokenResponse`**: Authenticate and obtain a token for an account.
  
  Example:
  
  ```python
  token_response = MailTMClient.get_token("mytempemail@mail.tm", "supersecretpassword")
  print(f"Token: {token_response.token}")
  ```

- **`MailTMClient.get_account(token: Optional[str] = None) -> Account`**: Get details of the authenticated account.
  
  Example:
  
  ```python
  account = client.get_account()
  print(f"Account: {account.address}")
  ```

- **`MailTMClient.get_account_by_id(account_id: str) -> Account`**: Get details of an account by its ID.
  
  Example:
  
  ```python
  account = client.get_account_by_id("some-account-id")
  print(f"Account: {account.address}")
  ```

- **`MailTMClient.delete_account(account_id: str) -> bool`**: Delete an account by its ID.
  
  Example:
  
  ```python
  success = client.delete_account("some-account-id")
  if success:
      print("Account successfully deleted.")
  ```

- **`MailTMClient.get_messages(page: int = 1) -> List[Message]`**: Retrieve messages for the authenticated account.
  
  Example:
  
  ```python
  messages = client.get_messages()
  for message in messages:
      print(f"From: {message.from_.address}, Subject: {message.subject}")
  ```

- **`MailTMClient.get_message_by_id(message_id: str) -> MessageDetail`**: Retrieve a specific message by its ID.
  
  Example:
  
  ```python
  message_detail = client.get_message_by_id("some-message-id")
  print(f"Message from: {message_detail.from_.address}, Subject: {message_detail.subject}")
  ```

- **`MailTMClient.delete_message(message_id: str) -> bool`**: Delete a specific message by its ID.
  
  Example:
  
  ```python
  success = client.delete_message("some-message-id")
  if success:
      print("Message successfully deleted.")
  ```

- **`MailTMClient.mark_message_as_read(message_id: str) -> MessageDetail`**: Mark a message as read.
  
  Example:
  
  ```python
  message_detail = client.mark_message_as_read("some-message-id")
  print(f"Message read status: {message_detail.seen}")
  ```

- **`MailTMClient.get_message_source(message_id: str) -> MessageSource`**: Get the raw source of a message.
  
  Example:
  
  ```python
  message_source = client.get_message_source("some-message-id")
  print(f"Message source: {message_source.data}")
  ```

- **`MailTMClient.get_message_attachments(message_id: str) -> List[Attachment]`**: Get the attachments of a specific message.
  
  Example:
  
  ```python
  attachments = client.get_message_attachments("some-message-id")
  for attachment in attachments:
      print(f"Attachment: {attachment.filename}")
  ```

- **`MailTMClient.get_attachment(message_id: str, attachment_id: str) -> Attachment`**: Get a specific attachment by its ID.
  
  Example:
  
  ```python
  attachment = client.get_attachment("some-message-id", "some-attachment-id")
  print(f"Attachment filename: {attachment.filename}")
  ```

## Contributing

We welcome contributions to the MailTM Python SDK! If you find a bug or want to add a feature, feel free to open an issue or submit a pull request.

## License

This project is licensed under the GNU General Public License v3 (GPLv3) - see the [LICENSE](./LICENSE) file for details.

---

**Note**: This SDK is designed for use with the MailTM API. Please ensure you use this SDK responsibly and adhere to MailTM's terms of service. Misuse of this SDK for illegal activities is strictly prohibited.