# Disposable Email Providers List

> A comprehensive, curated collection of disposable and temporary email provider domains for spam prevention, privacy protection, and email validation.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Last Updated](https://img.shields.io/github/last-commit/email-check-app/disposable-email-providers)](https://github.com/email-check-app/disposable-email-providers/commits/master)
[![Size](https://img.shields.io/github/repo-size/email-check-app/disposable-email-providers)](https://github.com/email-check-app/disposable-email-providers)

## 📋 Overview

This repository maintains an extensive, regularly updated list of disposable/temporary email provider domains. The data is aggregated from multiple reputable sources and carefully deduplicated to provide the most comprehensive collection available for:

- **Email Validation**: Block disposable emails during user registration
- **Spam Prevention**: Filter out temporary email addresses
- **Compliance**: Meet anti-fraud and marketing requirements
- **Development**: Test email functionality without using real addresses

## 📊 Statistics

- **Total Domains**: 259,095+ unique disposable email domains
- **JSON Format**: 169,981 structured entries
- **Regular Updates**: Frequently maintained with new providers
- **NPM Package**: Available as `@emailcheck/disposable-email-providers`

## 📁 Available Data

### JSON Format
```bash
disposable-email-providers.json
# Size: ~3.3MB
# Entries: 169,981
# Format: Array of domain strings
```

**Usage Example:**
```javascript
const fs = require('fs');
const disposableDomains = JSON.parse(fs.readFileSync('disposable-email-providers.json', 'utf8'));

function isDisposable(email) {
  const domain = email.split('@')[1].toLowerCase();
  return disposableDomains.includes(domain);
}
```

### NPM Package Installation
```bash
npm install @emailcheck/disposable-email-providers
```

**Usage with NPM:**
```javascript
const disposableDomains = require('@emailcheck/disposable-email-providers');

function isDisposable(email) {
  const domain = email.split('@')[1].toLowerCase();
  return disposableDomains.includes(domain);
}
```

## 🚀 Quick Start

### JavaScript/Node.js
```javascript
// Using JSON format
const fs = require('fs');
const disposableEmails = JSON.parse(fs.readFileSync('disposable-email-providers.json', 'utf8'));

function validateEmail(email) {
  const domain = email.split('@')[1]?.toLowerCase();
  return !disposableEmails.includes(domain);
}

// Example
console.log(validateEmail('user@gmail.com')); // true
console.log(validateEmail('user@10minutemail.com')); // false
```

### Python
```python
import json

# Load disposable domains
with open('disposable-email-providers.json', 'r') as f:
    disposable_domains = set(json.load(f))

def is_valid_email(email):
    try:
        domain = email.split('@')[1].lower()
        return domain not in disposable_domains
    except IndexError:
        return False

# Example
print(is_valid_email('user@gmail.com'))  # True
print(is_valid_email('user@tempmail.org'))  # False
```

### PHP
```php
<?php
// Load disposable domains
$disposableDomains = json_decode(file_get_contents('disposable-email-providers.json'), true);

function isDisposableEmail($email) {
    $domain = strtolower(substr(strrchr($email, '@'), 1));
    return in_array($domain, $disposableDomains);
}

// Example
var_dump(isDisposableEmail('user@gmail.com'));  // bool(false)
var_dump(isDisposableEmail('user@10minutemail.com'));  // bool(true)
```

## 🔄 Integration Examples

### Using EMAIL-CHECKER.APP Email Validator (Recommended)
For a complete email validation solution, consider using [email-validator-js](https://github.com/email-check-app/email-validator-js/), which includes this disposable email list along with comprehensive validation:

```bash
npm install @emailcheck/email-validator-js
```

```javascript
const { EmailValidator } = require('@emailcheck/email-validator-js');

const validator = new EmailValidator();

async function validateEmail(email) {
  const result = await validator.validate(email);
  return {
    isValid: result.isValid,
    isDisposable: result.isDisposable,
    score: result.score,
    details: result
  };
}

// Example usage
const result = await validateEmail('user@example.com');
console.log(result); // { isValid: true, isDisposable: false, score: 95, ... }
```

### Cloud API Service
For production applications, use the managed email validation service at [email-check.app](https://email-check.app):

```javascript
const response = await fetch('https://api.email-check.app/validate', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_API_KEY'
  },
  body: JSON.stringify({ email: 'user@example.com' })
});

const result = await response.json();
```

### Express.js Middleware
**Using NPM Package:**
```javascript
const express = require('express');
const disposableDomains = require('@emailcheck/disposable-email-providers');

const app = express();

// Middleware to block disposable emails
app.use((req, res, next) => {
  if (req.body.email) {
    const domain = req.body.email.split('@')[1]?.toLowerCase();
    if (disposableDomains.includes(domain)) {
      return res.status(400).json({ error: 'Disposable email addresses are not allowed' });
    }
  }
  next();
});

app.post('/register', (req, res) => {
  // Registration logic here
  res.json({ success: true });
});
```

**Using Local File:**
```javascript
const express = require('express');
const fs = require('fs');
const disposableDomains = JSON.parse(fs.readFileSync('./disposable-email-providers.json', 'utf8'));

const app = express();

// Middleware to block disposable emails
app.use((req, res, next) => {
  if (req.body.email) {
    const domain = req.body.email.split('@')[1]?.toLowerCase();
    if (disposableDomains.includes(domain)) {
      return res.status(400).json({ error: 'Disposable email addresses are not allowed' });
    }
  }
  next();
});
```

### Laravel Validation Rule
```php
<?php

namespace App\Rules;

use Closure;
use Illuminate\Contracts\Validation\ValidationRule;

class NotDisposableEmail implements ValidationRule
{
    public function validate(string $attribute, mixed $value, Closure $fail): void
    {
        // Using local file
        $disposableDomains = json_decode(file_get_contents(storage_path('app/disposable-email-providers.json')), true);

        // Or using NPM package data (if you have it published)
        // $disposableDomains = include base_path('vendor/emailcheck/disposable-email-providers/disposable-email-providers.json');

        $domain = strtolower(substr(strrchr($value, '@'), 1));

        if (in_array($domain, $disposableDomains)) {
            $fail('Disposable email addresses are not allowed.');
        }
    }
}
```

## 🛠️ Maintenance

The list is regularly updated through automated processes:

- **Deduplication**: Merge and remove duplicates from multiple sources
- **Validation**: Verify domain accessibility and functionality
- **Sorting**: Maintain organized, searchable lists
- **Regular Updates**: Weekly/monthly updates with new providers

## 📝 License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

### How to Contribute

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/new-providers`)
3. Add your changes to the disposable-email-providers.json file
4. Commit your changes (`git commit -m 'Add new disposable email providers'`)
5. Push to the branch (`git push origin feature/new-providers`)
6. Open a Pull Request

## ⚠️ Important Notes

- **Regular Updates**: Disposable email providers appear frequently. Regular updates are recommended
- **False Positives**: Some legitimate services might be flagged. Consider implementing an allowlist
- **Performance**: For high-traffic applications, consider loading the data into memory or using a database
- **Legal Compliance**: Ensure compliance with local regulations regarding email validation

## 🔍 Related Projects

### Cloud Solutions
- [email-validator-js](https://github.com/email-check-app/email-validator-js/) - **Complete email validation library** with disposable email detection, syntax validation, and deliverability checks
- [email-check.app](https://email-check.app) - **Managed cloud API** for email validation with high performance and reliability

## 📞 Support

For questions, issues, or suggestions:

- 🐛 [Report Issues](https://github.com/email-check-app/disposable-email-providers/issues)
- 💬 [Discussions](https://github.com/email-check-app/disposable-email-providers/discussions)
- 📧 [Maintainer: EMAIL-CHECK.APP](https://email-check.app)
- 🌐 [Email Validator Service](https://email-check.app) - For production email validation needs

---

**⭐ If this project helps you, consider giving it a star!**
