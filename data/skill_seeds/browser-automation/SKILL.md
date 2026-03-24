---
name: browser-automation
description: "Automate browser workflows using Playwright, Puppeteer, or Selenium. Use when scraping web data, filling forms programmatically, capturing screenshots or PDFs, intercepting network requests, or building headless browser automation scripts. For end-to-end testing, use the e2e-playwright skill instead."
---

# Browser Automation

## Workflow

1. **Launch browser** -- Choose headless/headful mode, set viewport and context
2. **Navigate and wait** -- Go to URL, wait for `networkidle` or specific selectors
3. **Select elements** -- Use `data-testid` (preferred), CSS selectors, or Playwright locators
4. **Perform actions** -- Click, fill, select, upload files, intercept network
5. **Extract or capture** -- Scrape data, take screenshots, generate PDFs
6. **Clean up** -- Close browser in `finally` block to prevent resource leaks

## Code Examples

### Basic Page Navigation
```typescript
import { chromium } from 'playwright';

const browser = await chromium.launch({ headless: true });
const page = await browser.newPage();
await page.goto('https://example.com', { waitUntil: 'networkidle' });
await page.screenshot({ path: 'screenshot.png', fullPage: true });
await browser.close();
```

### Form Automation with Validation
```typescript
// Fill and submit form
await page.fill('input[name="email"]', 'test@example.com');
await page.fill('input[name="password"]', 'SecurePass123!');
await page.click('button[type="submit"]');

// Wait for success message
const success = await page.waitForSelector('.success-message', { timeout: 5000 });
const message = await success.textContent();
console.log('Success:', message);
```

### Data Extraction from Multiple Pages
```typescript
const products = [];
let page = 1;

while (page <= 10) {
  await browser.goto(`https://example.com/products?page=${page}`);

  const items = await browser.$$eval('.product-item', (elements) =>
    elements.map((el) => ({
      title: el.querySelector('.title')?.textContent,
      price: el.querySelector('.price')?.textContent,
      image: el.querySelector('img')?.src,
    }))
  );

  products.push(...items);
  page++;
}
```

### Network Interception
```typescript
await page.route('**/api/analytics', (route) => route.abort());
await page.route('**/api/user', (route) =>
  route.fulfill({
    status: 200,
    body: JSON.stringify({ id: 1, name: 'Test User' }),
  })
);
```

### Visual Regression Testing
```typescript
import { expect } from '@playwright/test';

// Capture baseline
await page.screenshot({ path: 'baseline.png' });

// After changes, compare
const screenshot = await page.screenshot();
expect(screenshot).toMatchSnapshot('homepage.png');
```

## Selector Strategies

### 1. CSS Selectors (Preferred)
```typescript
// ID selector (most reliable)
await page.click('#submit-button');

// Data attribute (best practice)
await page.click('[data-testid="login-button"]');

// Class selector
await page.click('.btn-primary');

// Combined selector
await page.click('button.submit[type="submit"]');
```

### 2. XPath (When CSS isn't enough)
```typescript
// Text-based selection
await page.click('//button[contains(text(), "Submit")]');

// Complex hierarchy
await page.click('//div[@class="form"]//input[@name="email"]');
```

### 3. Playwright-Specific
```typescript
// Text-based
await page.getByText('Submit').click();

// Role-based (accessibility)
await page.getByRole('button', { name: 'Submit' }).click();

// Label-based
await page.getByLabel('Email address').fill('test@example.com');

// Placeholder-based
await page.getByPlaceholder('Enter your email').fill('test@example.com');
```

## Best Practices

### 1. Use Stable Selectors
❌ **Bad**: `.css-4j6h2k-button` (auto-generated class)
✅ **Good**: `[data-testid="submit-button"]`

### 2. Add Explicit Waits
❌ **Bad**: `await page.waitForTimeout(3000);`
✅ **Good**: `await page.waitForSelector('.results', { state: 'visible' });`

### 3. Handle Errors Gracefully
```typescript
try {
  await page.click('button', { timeout: 5000 });
} catch (error) {
  await page.screenshot({ path: 'error.png' });
  console.error('Click failed:', error.message);
  throw error;
}
```

### 4. Clean Up Resources
```typescript
try {
  // automation code
} finally {
  await browser.close();
}
```

### 5. Use Page Object Model (POM)
```typescript
class LoginPage {
  constructor(private page: Page) {}

  async login(email: string, password: string) {
    await this.page.fill('[data-testid="email"]', email);
    await this.page.fill('[data-testid="password"]', password);
    await this.page.click('[data-testid="submit"]');
  }

  async isLoggedIn() {
    return this.page.locator('[data-testid="dashboard"]').isVisible();
  }
}
```

## Common Pitfalls

### 1. Race Conditions
```typescript
// ❌ Race condition
await page.click('button');
const text = await page.textContent('.result'); // May fail!

// ✅ Wait for element
await page.click('button');
await page.waitForSelector('.result');
const text = await page.textContent('.result');
```

### 2. Stale Element References
```typescript
// ❌ Element may become stale
const element = await page.$('button');
await page.reload();
await element.click(); // Error: element detached from DOM

// ✅ Re-query after page changes
await page.reload();
await page.click('button');
```

### 3. Timing Issues with Dynamic Content
```typescript
// ❌ Assumes immediate load
await page.goto('https://example.com');
await page.click('.dynamic-content'); // May fail!

// ✅ Wait for dynamic content
await page.goto('https://example.com');
await page.waitForLoadState('networkidle');
await page.click('.dynamic-content');
```

## Debugging Tools

### 1. Headful Mode
```typescript
const browser = await chromium.launch({ headless: false, slowMo: 100 });
```

### 2. Screenshot on Failure
```typescript
test.afterEach(async ({ page }, testInfo) => {
  if (testInfo.status !== 'passed') {
    await page.screenshot({ path: `failure-${testInfo.title}.png` });
  }
});
```

### 3. Trace Recording
```typescript
await context.tracing.start({ screenshots: true, snapshots: true });
await page.goto('https://example.com');
// ... automation steps
await context.tracing.stop({ path: 'trace.zip' });
```

### 4. Console Logs
```typescript
page.on('console', (msg) => console.log('Browser log:', msg.text()));
```

## Performance Optimization

### 1. Block Unnecessary Resources
```typescript
await page.route('**/*.{png,jpg,jpeg,gif,svg,css}', (route) => route.abort());
```

### 2. Reuse Browser Contexts
```typescript
const context = await browser.newContext();
const page1 = await context.newPage();
const page2 = await context.newPage();
// Share cookies, storage, etc.
```

### 3. Parallel Execution
```typescript
await Promise.all([
  page1.goto('https://example.com/page1'),
  page2.goto('https://example.com/page2'),
  page3.goto('https://example.com/page3'),
]);
```

## Related Skills

- `e2e-playwright` -- End-to-end testing with Playwright Test
- `frontend` -- Understanding DOM structure
- `api-testing` -- Mocking network requests
