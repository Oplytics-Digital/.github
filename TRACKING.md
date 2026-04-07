# Platform Tracker

Tracks all platform changes across Oplytics Digital repositories.

## Completed

| # | Action | Repo | PR / Ref | Merged / Published | Sign Off |
|---|--------|------|----------|--------------------|----------|
| 1 | Fix "Explore Solutions" anchor + remove "See How It Works" buttons | oplytics-marketing-site | [#70](https://github.com/Oplytics-Digital/oplytics-marketing-site/pull/70) | ✅ Merged | |
| 2 | Fix SQDCP hero (key mismatch, subheadlines, serviceFeatures) | oplytics-marketing-site | [#71](https://github.com/Oplytics-Digital/oplytics-marketing-site/pull/71) | ✅ Merged | |
| 3 | Fix expiring manuscdn.com demo image URLs | oplytics-marketing-site | [#72](https://github.com/Oplytics-Digital/oplytics-marketing-site/pull/72) | ✅ Merged | |
| 4 | Update Policy Deployment demo with real CloudFront screenshot | oplytics-marketing-site | [#73](https://github.com/Oplytics-Digital/oplytics-marketing-site/pull/73) | ✅ Merged | |
| 5 | Update SQDCP demo to latest CloudFront screenshot | oplytics-marketing-site | [#74](https://github.com/Oplytics-Digital/oplytics-marketing-site/pull/74) | ✅ Merged | |
| 6 | Remove T4→P1.1 cross-BU correlation + add BU validation guard | oplytics-policy-deployment | [#27](https://github.com/Oplytics-Digital/oplytics-policy-deployment/pull/27) | ✅ Merged | |
| 7 | Publish SQDCP pillar constants (`@pablo2410/core-server@0.2.1`) | oplytics-core-server | npm publish | ✅ Published | |
| 8 | Migrate project/BO categories to SQDCP pillars | oplytics-policy-deployment | [#27](https://github.com/Oplytics-Digital/oplytics-policy-deployment/pull/27) | ✅ Merged | |
| 9 | Add cascade required indicators to Catchball BO/AO rows | oplytics-policy-deployment | [#28](https://github.com/Oplytics-Digital/oplytics-policy-deployment/pull/28) | ✅ Merged | |
| 10 | Rename project codes to BU-prefixed format (FB/FM/ES/ENT) | oplytics-policy-deployment | [#29](https://github.com/Oplytics-Digital/oplytics-policy-deployment/pull/29) | ✅ Merged | |
| 11 | Update Paul Cox platform_admin email to paul@oplytics.digital | oplytics-portal | [#9](https://github.com/Oplytics-Digital/oplytics-portal/pull/9) | ✅ Merged | |

## In Progress / Open

| # | Action | Repo | PR / Ref | Merged / Published | Sign Off |
|---|--------|------|----------|--------------------|----------|
| 12 | Group users by role in admin panel users tab | oplytics-portal | [#10](https://github.com/Oplytics-Digital/oplytics-portal/pull/10) | ⏳ Open | |
| 13 | Remove `dashboard /` page label from header | oplytics-policy-deployment | [#40](https://github.com/Oplytics-Digital/oplytics-policy-deployment/pull/40) | ⏳ Open | |

## Role Architecture — Decisions & Cleanup Required

| # | Action | Repo | PR / Ref | Status | Sign Off |
|---|--------|------|----------|--------|----------|
| 14 | **Decision:** Define purpose of `superuser` role vs `enterprise_admin` — is it permanent or legacy? | oplytics-portal | — | 🔴 Decision needed | |
| 15 | Remove `'admin'` string from `SharedSidebar` `ADMIN_ROLES` — dead code, no user ever has this role | oplytics-portal | — | 🔧 Pending | |
| 16 | Move `liselorekolff@gmail.com` role from hardcoded `oauth.ts` + `seed-passwords.mjs` to database-driven assignment | oplytics-portal | — | 🔧 Pending | |
| 17 | **Decision:** Should `enterprise_admin` be able to promote users to `superuser`? Currently they can via `setRole` mutation. | oplytics-portal | — | 🔴 Decision needed | |

## Policy Deployment Phase 2 — Portal Integration Blockers

| # | Action | Repo | PR / Ref | Status | Sign Off |
|---|--------|------|----------|--------|----------|
| 18 | Portal tRPC router is not callable from subdomains — uses JWT cookie auth; subdomains use `X-Service-Key`. Write operations must go via the service API. | oplytics-portal | — | 🔴 Blocker | |
| 19 | Add write endpoints (`POST`/`PUT`/`DELETE`) to `policyServiceApi.ts` for plans, objectives, projects, KPIs, correlations, bowling entries, and deployment targets | oplytics-portal | — | 🔧 Pending | |
| 20 | Fix `listPushLogs` enterprise scoping bug — returns all push logs unfiltered regardless of enterprise context | oplytics-portal | — | 🔧 Pending | |
| 21 | Once write endpoints exist: migrate policy-deployment subdomain from local DB to portal as single source of truth | oplytics-policy-deployment | — | ⏳ Blocked on #19 | |

## Subdomain Onboarding Checklist

Steps required to onboard a new subdomain to the portal platform:

- [ ] Generate per-enterprise service API key (`oply_*`) via portal admin and set as `PORTAL_API_KEY` env var on subdomain
- [ ] Set `PORTAL_URL` env var on subdomain (`https://portal.oplytics.digital`)
- [ ] Implement `portalClient.ts` — authenticated Axios client with `X-Service-Key` header and graceful degradation
- [ ] Wire `GET /api/service/hierarchy` to populate org hierarchy context in subdomain
- [ ] Wire `GET /api/service/users/by-openid/:openId` for SSO user resolution at login
- [ ] Add subdomain origin to portal CORS whitelist in `server/_core/index.ts`
- [ ] Register subdomain in `userServices` table (slug + display name)
- [ ] Add subdomain to `PLATFORM_ADMIN_ONLY_SERVICES` if access should be restricted
