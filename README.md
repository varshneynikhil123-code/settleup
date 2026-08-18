# SettleUp

A group expense splitter that figures out who owes whom, in as few payments as possible.

## Why I'm building this

Every trip ends the same way. One person paid for the hotel, someone else covered dinner twice, another one filled petrol on the way back, and nobody wrote any of it down. Then everyone sits in a room scrolling through UPI history trying to reconstruct what happened.

We usually end up doing it on a calculator, getting it slightly wrong, and someone quietly loses ₹200.

I wanted to fix that for my own group first. Everything else came after.

## What it does

You make a group, add the people in it, and log expenses as they happen: who paid, how much, and who it should be split between.

Not every expense splits evenly, so there are three ways to divide one:

- **Equal** — split across everyone in the group
- **Exact** — you type each person's share yourself
- **Percentage** — useful when three people share a room and one gets the couch

Once the expenses are in, the app works out where everyone stands and tells you the exact payments needed to settle up.

## The part that's actually interesting

Most of this app is forms and lists. The one real problem is reducing the number of payments.

Take three people. Amit owes Riya ₹500. Riya owes Karan ₹500.

The obvious answer is two payments. But Riya is just a middleman here — the money arrives and leaves again. The real answer is one payment: Amit pays Karan ₹500, and Riya is done without touching anything.

That's a small example. In a group of six, a naive settlement can produce up to 15 separate payments. This approach brings it down to 5 at most.

## How the settlement works

First, each person gets a net balance:

```
balance = (everything they paid) − (their share of everything)
```

Positive means they're owed money. Negative means they owe. The sum of all balances is always zero, which is a useful thing to assert against — if it isn't zero, something upstream is broken.

Then it runs like this:

1. Take the person owed the most, and the person who owes the most
2. Settle whichever of the two amounts is smaller
3. Whoever hits zero drops out
4. Repeat until nobody is left

Since at least one person is removed every round, this terminates in at most **n−1** payments for n people. The naive pairwise approach can produce **n(n−1)/2**.

It's a greedy algorithm, and worth being honest about: greedy gives you n−1, which is a good bound, but it isn't always the theoretical minimum. Finding the true minimum is NP-hard, and n−1 is close enough that nobody in a real group would notice the difference.

## Built with

Plain HTML, CSS and JavaScript. No framework, no build step, no backend. Data sits in `localStorage`, so it works offline and nothing ever leaves the browser.

The vanilla part is on purpose. I wanted to understand the DOM, event handling and state properly before picking up React, since it's much easier to appreciate what a framework does for you once you've done it the hard way.

## Status

Still building. Roughly in this order:

- [ ] Group creation, add and remove members
- [ ] Expense entry with equal split
- [ ] Exact and percentage splits
- [ ] Balance calculation
- [ ] Settlement algorithm
- [ ] Expense history with search and filters
- [ ] Spending breakdown by category
- [ ] localStorage persistence
- [ ] JSON export and import
- [ ] Responsive layout

I'll keep this list updated as things land.

## Running it

```
git clone https://github.com/<varshneynikhil123-code>/settleup.git
cd settleup
```

Open `index.html` in a browser. There's nothing to install.

## Notes

Third semester project. Built alone, in roughly an hour a day.

If you spot a bug in the settlement logic, please open an issue — that's the part I most want to get right.
## Documentation

[Software Requirements Specification (PDF)](docs/SettleUp-SRS-v2.3.pdf)
