# 📚 A Blockchain-Based Library Lending System

A decentralized library management system built on the Ethereum blockchain (Sepolia Testnet). It enables librarians to add books, lend them to borrowers, and track availability — all enforced transparently via a smart contract, with no central authority required.

---

## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Smart Contract | Solidity `^0.8.0` |
| Blockchain Network | Ethereum Sepolia Testnet |
| Frontend | HTML, CSS, Vanilla JavaScript |
| Web3 Library | [ethers.js v5.7.2](https://docs.ethers.org/v5/) |
| Wallet | MetaMask |

---

## ✨ Features

- **Add Books** — Any connected wallet can act as a librarian and register new books with a unique ID and title.
- **Lend Books** — The librarian who added a book can issue it to a borrower's Ethereum address.
- **Return Books** — The issuing librarian can mark a lent book as returned.
- **Check Availability** — Anyone can query whether a specific book is available or lent out.
- **Global Catalog** — Browse all registered books and their live availability status in a table.
- **Access Control** — The `onlyIssuer` modifier ensures only the librarian who registered a book can lend or return it.
- **Event Logging** — All state-changing actions (`BookAdded`, `BookLent`, `BookReturned`) emit on-chain events for full auditability.

---

## 📄 Smart Contract

**File:** `sol_2.txt`  
**Language:** Solidity `^0.8.0`  
**License:** MIT

### Book Struct

```solidity
struct Book {
    string bookId;       // Unique identifier for the book
    string title;        // Title of the book
    address borrower;    // Current borrower's address (0x0 if available)
    bool isAvailable;    // Availability flag
    address issuer;      // Librarian who registered the book
}
```

### Functions

| Function | Access | Description |
|---|---|---|
| `addBook(bookId, title)` | Public | Registers a new book; caller becomes the issuer |
| `lendBook(bookId, borrower)` | Issuer only | Marks a book as lent to the given address |
| `returnBook(bookId)` | Issuer only | Marks a lent book as returned |
| `checkAvailability(bookId)` | Public (view) | Returns `(isAvailable, title)` for a given book ID |
| `getMyIssuedBooks()` | Public (view) | Returns all books registered by the calling address |
| `books(bookId)` | Public (view) | Auto-generated getter for the books mapping |
| `allBookIds(index)` | Public (view) | Auto-generated getter to iterate all book IDs |

### Events

| Event | Trigger |
|---|---|
| `BookAdded(bookId, title, issuer)` | A new book is registered |
| `BookLent(bookId, borrower)` | A book is issued to a borrower |
| `BookReturned(bookId)` | A book is marked as returned |

---

## 🚀 Deployed Contracts (Sepolia Testnet)

| Deployer Account | Contract Address |
|---|---|
| `0xb7cb...45dc` | `0x73EFF0Ae3037f9C2104ed601A4B8cBbB1746E09a` |
| `0x2c79...48cb` | `0x6382738fc95BDd86d4C356D2A8c8254450417790` |

> The frontend (`index.html`) is currently configured to use contract `0x6382738fc95BDd86d4C356D2A8c8254450417790`.

---

## 🖥️ Running the Frontend

### Prerequisites

- [MetaMask](https://metamask.io/) browser extension installed
- MetaMask configured with the **Sepolia Testnet**
- Some Sepolia ETH (get from a faucet like [sepoliafaucet.com](https://sepoliafaucet.com/)) for gas fees

### Steps

1. **Clone the repository** or download the project files.
2. **Start a local HTTP server** from the project directory:
   ```bash
   python -m http.server 8000
   ```
3. **Open your browser** and navigate to:
   ```
   http://localhost:8000
   ```
4. **Connect MetaMask** — Click the `Connect MetaMask` button. The app will automatically prompt you to switch to Sepolia if needed.
5. **Interact** with the library system using the sections on the page.

> ⚠️ Opening `index.html` directly as a `file://` URL will not work due to browser security restrictions. Always use a local server.

---

## 📖 How to Use

1. **Add a Book** *(Librarian)*  
   Enter a unique Book ID (e.g., `101`) and a title, then click **Add to Library**. Your wallet address becomes the issuer.

2. **Lend a Book** *(Librarian)*  
   Enter the Book ID and the borrower's Ethereum address (`0x...`), then click **Issue Book**.

3. **Check Status / Return a Book**  
   Enter a Book ID and click **Check Status** to see its title and availability. Click **Mark as Returned** to return the book (issuer only).

4. **View Global Catalog**  
   Click **Refresh Catalog** to fetch and display all registered books with their availability status.

---

## 🔐 Security & Design Notes

- The `onlyIssuer` modifier enforces that only the librarian who added a book can modify its state (lend/return). This prevents unauthorized access.
- Book IDs must be unique — attempting to add a duplicate ID will revert the transaction.
- All data is stored on-chain, making the system transparent and tamper-proof.
- No admin/owner role exists; each librarian manages only their own books, satisfying a decentralized multi-librarian use case.

