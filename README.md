# Two-Phase Locking (2PL) with Wound-Wait deadlock prevention
==============================================================================
### This program simulates the behavior of the two-phase locking (2PL) protocol for concurrency control. The particular protocol to be implemented will be rigorous 2PL, with the wound-wait method for dealing with deadlock. The input to this program will be a file of transaction operations in a particular sequence. Each line has a single transaction operation. The possible operations are: b (begin transaction), r (read item), w (write item), and e (end transaction). Each operation will be followed by a transaction id that is an integer between 1 and 9. For r and w operations, an item name follows between parentheses (item names are single letters from A to Z). Two example are given below. (Important Note: The input sequence will be affected during the simulation – for example, some commands may cause a transaction to be blocked (wait). In such cases, the simulation should keep track of all operations of this transaction, and if the transaction is resumed, these operations should be simulated).

<br>

### Usage:
``` java -jar ConcurencyControl.jar input_filename.txt ```
<br>
### 
- ###  Simulation assigns transaction timestamps using an incremental counter, based on the transactions start order.
- ### Transaction Table keeps relevant information about each transaction. This includes transaction id, transaction timestamp, transaction state (active, blocked (waiting), aborted (cancelled), committed, etc.), list of items locked by the transaction, plus any other relevant information.
- ### For blocked transaction, simulation also keeps an ordered list of the operations of that transaction (from the input file) that are waiting to be executed once the transaction can be resumed.
- ### Lock Table keeps relevant information about each locked data item. This includes item name, lock state (read (share) locked, or write (exclusive) locked), transaction id for the transaction holding the lock (for write locked) or list of transaction ids for the transactions holding the lock (for read locked), list of transaction ids for transactions waiting for the item to be unlocked (if any), plus any other relevant information.
######
- ### Before processing a read operation (r(X)), the appropriate read lock(X) request is simulated by the program, and the lock table is updated appropriately. If the item is not locked, the operation is allowed and the lock table is updated to indicate that the requested item is now read-locked by the transaction.
- #### (i) If the item X is already locked by a conflicting write lock, the transaction is either: (i) blocked (wait) and its transaction state is changed to blocked (in the transaction table), or (ii) the simulation will abort the other transaction holding the write lock on item X (wound) and the state of that other transaction is changed to aborted. The deadlock prevention protocol (wound-wait) determines which action is taken.
- #### (ii) If the item is already locked by a non-conflicting read lock, the transaction is added to the list of transactions that hold the read lock on the requested item (in the lock table).
- ### Before processing a write operation (w(X)), the appropriate write lock(X) request is processed by program (lock upgrading is permitted if the upgrade conditions are met – that is, if the item is read locked by only the transaction that is requesting the write lock). The lock table is updated appropriately.
- #### (i) If the item is not locked, the operation is allowed and the lock table is updated to indicate that the requested item is now write-locked by the transaction.
- #### (ii) If the item is already locked by a conflicting read or write lock, the transaction is either: (i) blocked (wait) and its state is changed to blocked (in the transaction table), or (ii) the simulation will abort the other transaction holding the read or write lock on item X (wound) and the state of that other transaction is changed to aborted. The deadlock prevention protocol (wound-wait) determines which action is taken.
