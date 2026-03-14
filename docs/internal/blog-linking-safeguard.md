# Internal Engineering Note

Project: Learn Without Limits CIC Blog
Site: https://blog.learnwithoutlimitscic.org
Stack: GitHub Pages + Jekyll

Purpose: Preserve the March 2026 internal-linking failure and fix so that future contributors always use Jekyll `post_url` for internal blog links.

---

# LWL Blog Linking and Migration Safeguard Note

**Date:** 13 March 2026
**Project:** Learn Without Limits CIC blog (`blog.learnwithoutlimitscic.org`)

Purpose: Preserve the key lessons from the March 2026 blog migration, article cluster launch, and internal-linking issue so future threads do not lose context.

---

# What happened

During the March 2026 pre-briefing article sprint, five linked articles were published on the new GitHub Pages / Jekyll blog. The Eventbrite link worked, but the internal article-to-article links initially returned **404 errors**.

---

# Root cause

The internal links were written manually in `.html` format, for example:

```
[Example article](/2026/03/13/example-article.html)
```

However, the live site publishes posts using **directory-style permalinks with trailing slashes**, for example:

```
https://blog.learnwithoutlimitscic.org/2026/03/13/example-article/
```

Because of that mismatch, the internal article links broke while external links such as Eventbrite continued working.

---

# Correct fix

All affected internal article links were replaced using **Jekyll `post_url` tags**.

Correct pattern:

```
{% raw %}[Example article]({% post_url 2026-03-13-example-article %}){% endraw %}
```

This allows Jekyll to generate the correct live URL automatically, regardless of permalink format.

---

# Permanent rule from now on

## For internal blog post links

Always use:

```
{% raw %}{% post_url YYYY-MM-DD-filename-without-md %}{% endraw %}
```

Example:

```
{% raw %}[Why Support for Children with ALN Often Arrives Only After Crisis]({% post_url 2026-03-13-why-aln-support-arrives-after-crisis %}){% endraw %}
```

---

## For external links

Use normal full URLs.

Example:

```
[Register for the Learn Without Limits briefing](https://www.eventbrite.com/e/lwl-briefing-prevention-bridging-progression-in-the-aln-system-tickets-1985052696050)
```

---

# Never do this again

Do **not** hand-type blog post links in `.html` format.

Wrong pattern:

```
/article-name.html
```

---

# The key trick that prevents this ever happening again

Use **Jekyll `post_url` tags for every internal article link**.

This is the safest method because:

* it follows the real permalink structure automatically
* it survives future permalink changes better than manual URLs
* it reduces human error when linking clusters together
* it makes migrations and refactors safer

---

# Secondary safeguard

Before publishing or promoting a linked article cluster:

1. Open the flagship article live on the site.
2. Click every internal article link in the cluster.
3. Confirm each loads successfully.
4. Only then promote on LinkedIn, X, Facebook, Eventbrite, etc.

This should become the **cluster QA rule**.

---

# Pre-briefing cluster published on 13 March 2026

These five articles were linked as a deliberate series:

1. `2026-03-13-when-children-cannot-attend-school-wales.md`
2. `2026-03-13-why-aln-support-arrives-after-crisis.md`
3. `2026-03-13-when-communities-build-their-own-infrastructure.md`
4. `2026-03-13-from-facebook-community-to-knowledge-infrastructure.md`
5. `2026-03-13-when-communities-design-the-solution.md`

---

# Commit history pattern used to repair the issue

The repair workflow was:

* fix one file at a time
* commit one article at a time
* redeploy via GitHub Pages
* hard refresh / incognito test after deployment

Representative commit messages used:

* `Fix internal Jekyll links in children cannot attend school article`
* `Fix internal Jekyll links in ALN crisis article`
* `Fix internal Jekyll links in communities article`
* `Fix internal Jekyll links in Facebook to knowledge infrastructure article`
* `Fix internal Jekyll links in flagship article`

---

# Operational lesson

The GitHub Pages / Jekyll stack is the correct long-term choice for the LWL blog because it provides:

* stronger control over intellectual property
* version history and timestamp
