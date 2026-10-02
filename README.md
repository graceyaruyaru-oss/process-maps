# Process Maps

Three processes from my work as Business Systems & Operations Manager at a wholesale distributor, drawn so someone new could follow them on day one. Customer names, volumes and pricing are left out.

| Map | What it shows |
|---|---|
| [1. Order to invoice](#1-order-to-invoice) | How a retail customer's order moves from the electronic order feed to a sent invoice |
| [2. Cross-system reconciliation](#2-cross-system-reconciliation) | How I find and fix records that disagree across five systems |
| [3. Microsoft 365 rollout](#3-microsoft-365-rollout) | A 3-4 month company-wide rollout, from current-state review to go-live support |

---

## 1. Order to invoice

Orders arrive through the customer EDI feed, are checked against stock, shipped (sometimes in several parts), and invoiced. The dotted boxes are where most problems started.

```mermaid
flowchart LR
    A[Retail customer sends order<br/>through EDI feed] --> B[Order entered and checked:<br/>price, quantity, ship date]
    B --> C{Enough stock?}
    C -- Yes --> D[Warehouse picks and packs]
    C -- No --> E[Backorder created<br/>purchasing notified]
    E --> D
    D --> F{Full order shipped?}
    F -- No --> G[Partial shipment recorded<br/>open quantity carried forward]
    G --> D
    F -- Yes --> H[Shipment confirmed]
    H --> I[Invoice created in<br/>accounting system]
    I --> J[Invoice sent to customer]
    J --> K[Overdue invoices followed up]

    classDef risk stroke-dasharray: 5 5
    class E,G risk
```

**Where it went wrong, and why it mattered**
- **Partial shipments.** One order could ship in three to five parts. If any part was recorded in one system but not another, the open quantity was wrong everywhere downstream.
- **Backorders across two inventory environments** that had never been synchronized, so neither one could be trusted on its own.

---

## 2. Cross-system reconciliation

Five sources had to agree on the same order: the customer EDI feed, two inventory environments, the accounting ledger, the online store, and the warehouse's own records. This is the routine I used when they didn't.

```mermaid
flowchart TD
    A[Mismatch spotted:<br/>customer, price, quantity,<br/>shipment or invoice] --> B[Pull the record from<br/>every system that touches it]
    B --> C[Line them up side by side<br/>and find the first point<br/>where they disagree]
    C --> D{Which source is right?}
    D --> E[Confirm against the<br/>original document:<br/>PO, packing slip, invoice]
    E --> F[Correct the record<br/>in the system that was wrong]
    F --> G{Will it happen again?}
    G -- Yes --> H[Fix the handoff:<br/>update the SOP, the setting,<br/>or escalate to the vendor]
    G -- No --> I[Log it and close]
    H --> I
```

**The rule behind it:** fixing the record is half the job. The other half is changing the step that let it go wrong, so the same order doesn't need rebuilding next month.

---

## 3. Microsoft 365 rollout

A 3-4 month rollout for the whole company: 100+ account and credential entries reviewed, 10+ legacy computers, and scattered cloud resources.

```mermaid
flowchart LR
    subgraph P1[1. Current state]
        A1[Inventory accounts,<br/>devices and subscriptions]
        A2[Find duplicate and<br/>obsolete subscriptions]
    end
    subgraph P2[2. Design]
        B1[Plan Teams, OneDrive<br/>and SharePoint structure]
        B2[Set permission model:<br/>warehouse cannot see<br/>financial data]
    end
    subgraph P3[3. Migrate]
        C1[Move selected files<br/>and backups]
        C2[Set migration steps<br/>and cutover deadlines]
    end
    subgraph P4[4. Go live]
        D1[Train each team with guides,<br/>checklists and demos]
        D2[Support account, file and<br/>device issues after go-live]
    end
    P1 --> P2 --> P3 --> P4
```

**What it delivered**
- One place for company files and accounts, with a permission model that kept financial data away from warehouse staff.
- Duplicate subscriptions and obsolete payment arrangements retired along the way.
- Every team trained with guides, checklists, screenshots and demonstrations, then supported after go-live until they could work in the new environment on their own.
