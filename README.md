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

## 6. Install Maven

### For Ubuntu/Debian:
```bash
sudo apt update
sudo apt install maven
```

### For CentOS/RHEL/Fedora:
```bash
sudo yum install maven  # CentOS/RHEL
sudo dnf install maven  # Fedora
```

### Manual Installation (Recommended for specific versions):
```bash
# Download Maven (choose version from https://maven.apache.org/download.cgi)
cd /opt
sudo wget https://downloads.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz

# Extract
sudo tar -xvzf apache-maven-3.9.6-bin.tar.gz

# Rename directory (optional)
sudo mv apache-maven-3.9.6 maven
```

## 7. Set Maven Environment Variables

### Edit ~/.bashrc or ~/.bash_profile:
```bash
nano ~/.bashrc

# Add these lines:
export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH

# Save and reload
source ~/.bashrc
```

### For system-wide setup:
```bash
sudo nano /etc/profile.d/maven.sh

# Add:
export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH

sudo chmod +x /etc/profile.d/maven.sh
source /etc/profile.d/maven.sh
```

## 8. Verify Maven Setup

```bash
# Check Maven version
mvn -version

# Should show both Java and Maven info
```

## 9. Complete Example Configuration File (~/.bashrc)

```bash
# Java Configuration
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$PATH

# Maven Configuration
export MAVEN_HOME=/opt/maven
export PATH=$MAVEN_HOME/bin:$PATH

# Optional: Maven performance settings
export MAVEN_OPTS="-Xms1024m -Xmx2048m -XX:MaxPermSize=512m"
```

## 10. Troubleshooting Common Issues

### Issue: "java: command not found"
```bash
# Solution: Verify paths and reload
echo $PATH
ls -la $JAVA_HOME/bin/java
source ~/.bashrc
```

### Issue: Maven cannot find Java
```bash
# Check if JAVA_HOME is set correctly
echo $JAVA_HOME

# Update alternatives
sudo update-alternatives --config java
```

### Issue: Permission denied
```bash
# Check permissions
ls -la /usr/lib/jvm/

# Fix permissions if needed
sudo chmod -R 755 /usr/lib/jvm/java-11-openjdk-amd64
```

## 11. Switch Between Multiple Java Versions

```bash
# List available Java versions
sudo update-alternatives --config java

# Set default Java version
sudo update-alternatives --set java /usr/lib/jvm/java-17-openjdk-amd64/bin/java
```

## 12. Quick Setup Script

Create a setup script `setup_java_maven.sh`:

```bash
#!/bin/bash

# Update system
sudo apt update

# Install Java
sudo apt install openjdk-11-jdk -y

# Set Java environment
echo "export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64" >> ~/.bashrc
echo "export PATH=\$JAVA_HOME/bin:\$PATH" >> ~/.bashrc

# Install Maven
sudo apt install maven -y

# Alternative: Manual Maven install
# cd /opt
# sudo wget https://downloads.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
# sudo tar -xvzf apache-maven-3.9.6-bin.tar.gz
# sudo mv apache-maven-3.9.6 maven
# echo "export MAVEN_HOME=/opt/maven" >> ~/.bashrc
# echo "export PATH=\$MAVEN_HOME/bin:\$PATH" >> ~/.bashrc

# Reload bashrc
source ~/.bashrc

# Verify installation
echo "Java Version:"
java -version
echo -e "\nMaven Version:"
mvn -version
```

Make it executable:
```bash
chmod +x setup_java_maven.sh
./setup_java_maven.sh
```

## 13. Important Notes

1. **Path Priority**: The order in PATH matters. `$JAVA_HOME/bin:$PATH` places Java first
2. **Reload Required**: Always run `source ~/.bashrc` after making changes
3. **Check Installations**: Verify both Java and Maven are installed correctly
4. **Version Compatibility**: Ensure Maven version is compatible with your Java version
5. **System Reboot**: For system-wide changes, a reboot may be necessary

## 14. Testing Your Setup

Create a simple Maven project to test:
```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=my-app \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DinteractiveMode=false

cd my-app
mvn clean compile
mvn package
```

This completes the Java and Maven setup on Linux. The configuration will persist across reboots and terminal sessions.
