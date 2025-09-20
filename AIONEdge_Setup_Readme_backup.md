# AIONEdge Langflow Setup Guide

This guide provides step-by-step instructions for setting up and running Langflow on Windows for AIONEdge development.

## 📋 Prerequisites

- **Operating System**: Windows 10/11
- **Python**: 3.10 - 3.13 (Currently using Python 3.11.5)
- **Package Manager**: uv (Currently using uv 0.7.9)

## 🚀 Quick Start

### 1. Verify Prerequisites

Check if you have the required tools installed:

```powershell
# Check Python version
python --version

# Check uv version
uv --version
```

### 2. Install Langflow

```powershell
# Install Langflow with all dependencies
uv pip install langflow -U
```

### 3. Fix Windows-Specific Issues

**Important**: On Windows, you may encounter an onnxruntime DLL loading error. Fix it with:

```powershell
# Uninstall problematic onnxruntime
pip uninstall onnxruntime -y

# Install compatible version
pip install onnxruntime==1.16.3
```

### 4. Start Langflow

```powershell
# Start Langflow server
langflow run
```

The server will start and display:
```
+ Initializing Langflow...
+ Checking Environment...  
+ Starting Core Services...
+ Connecting Database...
+ Loading Components...     
+ Adding Starter Projects...
+ Launching Langflow...  

┌─────────────────────────────────────────────────────────────────────────┐
│                                                                         │
│  Welcome to Langflow                                                    │
│                                                                         │ 
│  * GitHub: Star for updates -> https://github.com/langflow-ai/langflow  │ 
│  # Discord: Join for support -> https://discord.com/invite/EqksyE2EX9   │ 
│                                                                         │ 
│  We collect anonymous usage data to improve Langflow.                   │ 
│  To opt out, set: DO_NOT_TRACK=true in your environment.                │ 
│                                                                         │ 
│  [OK] Open Langflow -> http://localhost:7860                            │ 
│                                                                         │ 
└─────────────────────────────────────────────────────────────────────────┘ 

INFO:     Started server process [XXXXX]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://localhost:7860
(Press CTRL+C to quit)
```

### 5. Access Langflow

Open your web browser and navigate to:
**http://localhost:7860**

## 🛠️ Advanced Configuration

### Custom Host and Port

```powershell
# Run on specific host and port
langflow run --host 0.0.0.0 --port 8080
```

### Environment Variables

```powershell
# Disable anonymous usage tracking
$env:DO_NOT_TRACK = "true"
langflow run
```

### Development Mode

For development with the source code:

```powershell
# Navigate to langflow directory
cd langflow

# Install dependencies
uv sync --frozen

# Run with uvicorn directly
uv run uvicorn --factory langflow.main:create_app --host 127.0.0.1 --port 7860 --reload
```

## 🛑 Stopping Langflow

### Method 1: Graceful Shutdown (Recommended)
In the terminal where Langflow is running:
```
Press CTRL+C
```

### Method 2: Force Kill Process
```powershell
# Find Langflow process
Get-Process | Where-Object {$_.ProcessName -like "*python*" -and $_.MainWindowTitle -like "*langflow*"}

# Kill specific process by ID (replace XXXXX with actual process ID)
Stop-Process -Id XXXXX -Force

# Or kill all Python processes (use with caution)
Get-Process python | Stop-Process -Force
```

### Method 3: Kill by Port
```powershell
# Find process using port 7860
netstat -ano | findstr :7860

# Kill process by PID (replace XXXXX with actual PID)
taskkill /PID XXXXX /F
```

## 🔧 Troubleshooting

### Issue: onnxruntime DLL Error
**Error**: `ImportError: DLL load failed while importing onnxruntime_pybind11_state`

**Solution**:
```powershell
pip uninstall onnxruntime -y
pip install onnxruntime==1.16.3
```

### Issue: Port Already in Use
**Error**: `Port 7860 is already in use`

**Solutions**:
```powershell
# Option 1: Use different port
langflow run --port 8080

# Option 2: Kill process using the port
netstat -ano | findstr :7860
taskkill /PID XXXXX /F
```

### Issue: Server Won't Start
**Check**:
1. Verify Python version compatibility
2. Ensure all dependencies are installed
3. Check if antivirus is blocking the application
4. Try running as administrator

### Issue: Browser Can't Connect
**Check**:
1. Ensure server is running (check terminal output)
2. Try `http://127.0.0.1:7860` instead of `http://localhost:7860`
3. Check Windows Firewall settings
4. Verify no proxy settings are interfering

## 📊 Monitoring

### Check Server Status
```powershell
# Check if Langflow is running
Get-Process | Where-Object {$_.ProcessName -like "*python*"}

# Check port usage
netstat -an | findstr "7860"

# Test server response
Invoke-WebRequest -Uri "http://127.0.0.1:7860" -UseBasicParsing
```

### View Logs
Langflow logs are displayed in the terminal where you started the server. Look for:
- `INFO`: Normal operation messages
- `ERROR`: Error messages requiring attention
- `WARNING`: Non-critical issues

## 🔄 Updates

### Update Langflow
```powershell
# Update to latest version
uv pip install langflow -U

# Or using pip directly
pip install langflow --upgrade
```

### Update Dependencies
```powershell
# Update all dependencies
uv pip list --outdated
uv pip install --upgrade package-name
```

## 📁 File Locations

### Default Directories
- **Langflow Data**: `%USERPROFILE%\.langflow`
- **Logs**: Terminal output
- **Database**: SQLite database in Langflow data directory

### Configuration Files
- **Environment Variables**: Set in PowerShell session or system environment
- **Settings**: Managed through Langflow web interface

## 🆘 Support

### Getting Help
1. **Documentation**: https://docs.langflow.org
2. **GitHub Issues**: https://github.com/langflow-ai/langflow/issues
3. **Discord Community**: https://discord.com/invite/EqksyE2EX9

### Common Commands Reference

```powershell
# Start Langflow
langflow run

# Start with custom settings
langflow run --host 0.0.0.0 --port 8080

# Check version
langflow --version

# Get help
langflow --help

# Check if running
Get-Process python | Where-Object {$_.ProcessName -eq "python"}

# Stop server
# Press CTRL+C in terminal or use Stop-Process commands above
```

## ✅ Verification Checklist

Before considering setup complete, verify:

- [ ] Python 3.10+ installed and accessible
- [ ] uv package manager installed
- [ ] Langflow installed successfully
- [ ] onnxruntime compatibility issue resolved (if applicable)
- [ ] Server starts without errors
- [ ] Web interface accessible at http://localhost:7860
- [ ] Can create a simple flow in the interface
- [ ] Server stops cleanly with CTRL+C

---

**Last Updated**: September 20, 2025  
**Version**: Langflow 1.6.0  
**Environment**: Windows 10/11 with Python 3.11.5
