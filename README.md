# Java and Maven Path Setup in Linux - Detailed Guide

## 1. Check if Java is Already Installed

```bash
# Check Java version
java -version

# Check if Java is installed
which java
```

## 2. Install Java (If Not Installed)

### For Ubuntu/Debian:
```bash
# Update package list
sudo apt update

# Install OpenJDK (choose version)
sudo apt install openjdk-11-jdk  # For Java 11
sudo apt install openjdk-17-jdk  # For Java 17
sudo apt install openjdk-21-jdk  # For Java 21
```

### For CentOS/RHEL/Fedora:
```bash
# Install OpenJDK
sudo yum install java-11-openjdk-devel  # For Java 11
sudo dnf install java-17-openjdk-devel  # For Java 17 (Fedora)
```

## 3. Find Java Installation Path

```bash
# Locate Java installation
update-alternatives --config java
# or
readlink -f $(which java)
# or
whereis java

# Find JAVA_HOME path (usually one of these)
/usr/lib/jvm/java-11-openjdk-amd64
/usr/lib/jvm/java-11-openjdk
/usr/lib/jvm/java-17-openjdk-amd64
/usr/java/jdk-11.0.xx
```

## 4. Set Java Environment Variables

### Temporary Setup (for current session only):
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

### Permanent Setup:

#### Option A: Edit ~/.bashrc or ~/.bash_profile (User-specific)
```bash
# Open the file
nano ~/.bashrc
# or
nano ~/.bash_profile

# Add these lines at the end:
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH

# Save and reload
source ~/.bashrc
# or
source ~/.bash_profile
```

#### Option B: System-wide Setup (in /etc/profile.d/)
```bash
# Create a new file
sudo nano /etc/profile.d/java.sh

# Add these lines:
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH

# Make it executable
sudo chmod +x /etc/profile.d/java.sh

# Reload the profile
source /etc/profile.d/java.sh
```

## 5. Verify Java Setup

```bash
# Check JAVA_HOME
echo $JAVA_HOME

# Verify Java version
java -version

# Check javac compiler
javac -version
```

## 6. Install JAVA And Maven

```bash
#!/bin/bash

# Update system
sudo apt update

# Install prerequisites
sudo apt install -y wget tar

# =========================
# Install OpenJDK 17
# =========================
JAVA_VERSION="17"
sudo apt install -y openjdk-${JAVA_VERSION}-jdk

# Set JAVA_HOME environment variable in ~/.bashrc
echo "export JAVA_HOME=/usr/lib/jvm/java-${JAVA_VERSION}-openjdk-amd64" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc

# =========================
# Download and install Maven manually
# =========================
MAVEN_VERSION="3.9.14"
MAVEN_TAR="apache-maven-${MAVEN_VERSION}-bin.tar.gz"
MAVEN_URL="https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/${MAVEN_TAR}"

# Download Maven tarball
wget -O /tmp/${MAVEN_TAR} ${MAVEN_URL}

# Check if download succeeded
if [ $? -ne 0 ]; then
  echo "Failed to download Maven. Please check the URL."
  exit 1
fi

# Extract to /opt
sudo tar -xvzf /tmp/${MAVEN_TAR} -C /opt

# Remove existing symlink if it exists
if [ -L /opt/maven ]; then
  sudo rm /opt/maven
fi

# Create a new symlink
sudo ln -s /opt/apache-maven-${MAVEN_VERSION} /opt/maven

# Set MAVEN_HOME environment variable in ~/.bashrc
echo "export MAVEN_HOME=/opt/maven" >> ~/.bashrc
echo "export PATH=\$MAVEN_HOME/bin:\$PATH" >> ~/.bashrc

# Cleanup
rm /tmp/${MAVEN_TAR}

# =========================
# Reload environment variables in current shell
# =========================
source ~/.bashrc

# =========================
# Verify installations
# =========================
echo "Java Version:"
java -version

echo -e "\nMaven Version:"
mvn -version

echo "Please run 'source ~/.bashrc' or restart your terminal to use Maven."

```
