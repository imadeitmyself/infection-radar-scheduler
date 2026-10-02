# Infection Radar refresh timer

This repository contains only the public timer for https://infection-radar.vercel.app. Application source, clinical archives, and owner credentials are held separately.

Standard `ubuntu-latest` runners in public repositories are free under [GitHub's billing policy](https://docs.github.com/en/billing/concepts/product-billing/github-actions). This workflow has no caches, uploaded artifacts, paid runners, dependencies, or application checkout.

Every 15 minutes, an authenticated request asks the production server to dispatch the currently due 00:00 or 12:00 Europe/London collection slot. The server clock and durable ledger prevent duplicate collections across delays and GMT/BST transitions. GitHub schedules can be delayed; the timer is not a timing SLA.

`RADAR_SCHEDULER_TOKEN` is a repository secret accepted only by `POST /ops/schedule`. It cannot read operations status, change content, trigger manual collections, or recover/roll back data. Dispatch has no GitHub token permissions. The separate weekly maintenance job has repository write permission only to record activity and prevent the public scheduler's 60-day inactivity disablement; it receives no scheduler secret.

Cloudflare Workers and D1 remain on their enforced Free allowances, and Vercel remains on Hobby. Exhausted limits must defer collection; never upgrade or add spending automatically.
