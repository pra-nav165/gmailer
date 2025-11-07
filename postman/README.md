# Gmail Bulk Sender Application

A complete, object-oriented Java application for sending bulk, personalized emails via the Gmail SMTP server. The application emphasizes separation of concerns and utilizes the Jakarta Mail API.

## Features

- **Graphical User Interface**: Modern, intuitive GUI for easy email management
- **Command-Line Interface**: Traditional CLI mode for automation and scripting
- **Secure Email Transmission**: Uses STARTTLS on port 587 for encrypted email delivery
- **Gmail App Password Support**: Requires Gmail App Password for enhanced security
- **Object-Oriented Design**: Clean separation of concerns with dedicated classes
- **HTML Email Support**: Send rich HTML-formatted emails
- **Error Handling**: Individual email failures don't stop the entire bulk operation
- **File-Based Configuration**: Easy configuration via properties file or GUI
- **Real-Time Progress Tracking**: Visual progress bar and status updates
- **Logging**: Comprehensive logging for monitoring email send status

## Project Structure

```
gmail-bulk-sender/
├── pom.xml                          # Maven build configuration
├── config.properties                # SMTP configuration file
├── recipients.txt                   # Recipient email addresses
├── README.md                        # This file
└── src/
    └── main/
        └── java/
            └── com/
                └── bulksender/
                    ├── BulkSenderApp.java      # Main entry point
                    ├── EmailConfig.java         # Configuration management
                    ├── EmailSender.java         # Core email sending logic
                    └── RecipientManager.java    # Recipient data handling
```

## Prerequisites

- Java 11 or higher
- Maven 3.6 or higher
- Gmail account with 2-Step Verification enabled
- Gmail App Password (see setup instructions below)

## Setup Instructions

### 1. Generate Gmail App Password

**IMPORTANT**: You must use a Gmail App Password, NOT your main Gmail password.

1. Go to your [Google Account settings](https://myaccount.google.com/)
2. Navigate to **Security** → **2-Step Verification**
3. Scroll down to **App passwords**
4. Select **Mail** as the app and **Other (Custom name)** as the device
5. Enter "Gmail Bulk Sender" as the custom name
6. Click **Generate**
7. Copy the 16-character password (no spaces)

### 2. Configure the Application

Edit `config.properties` and set your credentials:

```properties
sender.email=your-email@gmail.com
sender.password=your-16-character-app-password
```

### 3. Add Recipients

Edit `recipients.txt` and add one email address per line:

```
recipient1@example.com
recipient2@example.com
recipient3@example.com
```

Lines starting with `#` and empty lines are ignored.

## Building the Application

### Using Maven

```bash
# Compile the project
mvn clean compile

# Create executable JAR
mvn clean package
```

The executable JAR will be created in `target/gmail-bulk-sender-1.0.0.jar`.

## Running the Application

### GUI Mode (Recommended)

Launch the graphical user interface:

```bash
# Using Maven
mvn exec:java -Dexec.mainClass="com.bulksender.BulkSenderGUI"

# Or using the JAR file
java -jar target/gmail-bulk-sender-1.0.0.jar --gui
# or
java -jar target/gmail-bulk-sender-1.0.0.jar -g
```

The GUI provides:
- **Visual Configuration**: Enter SMTP settings directly in the interface
- **Recipient Management**: Load recipients from file or enter manually
- **Email Composition**: Rich text editor for subject and HTML body
- **Test Connection**: Verify your configuration before sending
- **Progress Tracking**: Real-time progress bar and status updates
- **Status Logs**: Detailed log of each email send operation

### Command-Line Mode

For automation and scripting:

```bash
# Using Maven
mvn exec:java -Dexec.mainClass="com.bulksender.BulkSenderApp"

# Using the JAR file
java -jar target/gmail-bulk-sender-1.0.0.jar
```

### Custom File Paths (CLI Mode)

You can specify custom paths for configuration and recipient files:

```bash
java -jar target/gmail-bulk-sender-1.0.0.jar my-config.properties my-recipients.txt
```

## Architecture

### EmailConfig.java
- Encapsulates SMTP configuration and credentials
- Loads configuration from properties files
- Provides JavaMail-compatible Properties objects
- Defaults to Gmail SMTP settings (smtp.gmail.com:587)

### RecipientManager.java
- Manages recipient email addresses
- Loads recipients from plain text files
- Supports comments and empty lines in recipient files
- Provides methods for recipient management

### EmailSender.java
- Handles JavaMail Session management
- Implements SMTP authentication
- Sends individual and bulk emails
- Includes comprehensive error handling
- Supports HTML email content

### BulkSenderApp.java
- Main entry point and orchestration
- Supports both GUI and CLI modes
- Loads configuration and recipients
- Initializes email sender
- Defines email content
- Executes bulk email operation

### BulkSenderGUI.java
- Graphical user interface for the application
- Provides intuitive form-based configuration
- File chooser for loading recipients
- Rich text composition area for email content
- Real-time progress tracking and status logging
- Connection testing functionality

## Security Best Practices

1. **Never commit credentials**: Add `config.properties` to `.gitignore`
2. **Use App Passwords**: Always use Gmail App Passwords, never main passwords
3. **Secure file permissions**: Restrict read access to configuration files
4. **Review recipients**: Verify recipient lists before sending bulk emails
5. **Rate limiting**: Be mindful of Gmail's sending limits (500 emails/day for free accounts)

## Gmail Sending Limits

- **Free Gmail accounts**: 500 emails per day
- **Google Workspace accounts**: 2000 emails per day
- **Rate limit**: ~100 emails per hour

The application will continue sending even if individual emails fail, allowing you to identify and retry failed recipients.

## Customization

### Custom Email Content

Edit the `buildEmailBody()` method in `BulkSenderApp.java` to customize the email template:

```java
private static String buildEmailBody() {
    return "<html>Your custom HTML content here</html>";
}
```

### Personalized Emails

You can extend `EmailSender.java` to support personalized content by replacing placeholders in the email body with recipient-specific data.

## Troubleshooting

### Authentication Failed
- Verify you're using a Gmail App Password, not your main password
- Ensure 2-Step Verification is enabled on your Google account
- Check that the App Password is correctly copied (no spaces)

### Connection Timeout
- Verify your internet connection
- Check firewall settings
- Ensure port 587 is not blocked

### No Recipients Loaded
- Verify `recipients.txt` exists and is readable
- Check that the file contains valid email addresses
- Ensure email addresses are on separate lines

## Dependencies

- **Jakarta Mail API 2.1.2**: Email API specification
- **Angus Mail 2.0.2**: Jakarta Mail implementation

## License

This project is provided as-is for educational and development purposes.

## Support

For issues related to:
- **Gmail authentication**: See [Google Account Help](https://support.google.com/accounts)
- **JavaMail**: See [Jakarta Mail Documentation](https://jakarta.ee/specifications/mail/)
- **Application bugs**: Review logs for detailed error messages

