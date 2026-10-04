# Store Shipping Policy Comparison

Turn two or three specified public shipping or after-sales policy pages into a traceable comparison of dispatch timing, service regions, fee conditions, and missing terms. Ambiguous store ownership or conflicting statements are reported as **undetermined**; the Skill never infers freight, delivery dates, or eligibility.

Input: a user-selected `browserId` and 2–3 public policy URLs. Output: a Markdown table with page-title/URL evidence and an explicit missing-items list. The default is read-only; an authorized draft fill never publishes or sends.

![Store shipping policy comparison promo](assets/promo.png)

## Install

```bash
npx skills add xxjrq/shop-shipping-policy-compare-free
```

## Browser prerequisite

Install [Easy WebBridge on GitHub](https://github.com/xxjrq/easy-webbridge), or use the [Gitee mirror](https://gitee.com/xxjrq/easy-webbridge). Connect the extension in the chosen browser profile, provide one `browserId`, and let this Skill create and own one task session only. It never broadcasts across accounts.

## Verification

```bash
node scripts/self-test.mjs
```

The test validates the independent repository structure, icon, and offline fixtures only. Fixtures are simulated page material and are not online collection results.

## Reference

The [Saleor repository](https://github.com/saleor/saleor) and its [README](https://github.com/saleor/saleor/blob/main/README.md) were consulted only for public e-commerce context; no source code or text implementation was copied. Licensed under [MIT](LICENSE).
