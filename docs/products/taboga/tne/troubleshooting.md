# Troubleshooting

## ❗ Common Issues

### 💾 Memory Management

**Issue**: Memory leaks after workbook creation
**Solution**: Ensure proper initialization and cleanup
```clarion
wBook.Init()
// ...operations...
wBook.Save('output.xls')
wBook.Kill()           !Important cleanup step
```

### 🎨 Formatting Issues

**Issue**: Unexpected cell formatting
**Solution**: Reset styles between operations
```clarion
wBook.ResetStyle()     !Clear previous formatting
```

### 📁 File Access

**Issue**: Cannot save workbook
**Solution**: Check file permissions and path
```clarion
IF NOT wBook.Save('C:\output.xls')
   MESSAGE('Error saving file - check permissions')
END
```

## ✨ Best Practices

1. Always initialize before use
2. Reset styles between operations
3. Clean up resources with Kill()
4. Use error handling for file operations
5. Check file paths and permissions

## 🤝 Getting Help

For technical assistance:
- Email: support@fomintools.com
- Include example code demonstrating the issue
- Provide Clarion version and OS details
