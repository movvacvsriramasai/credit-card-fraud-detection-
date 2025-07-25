import java.util.*;

class Transaction {
    String cardNumber;
    String location;
    double amount;
    Date timestamp;

    public Transaction(String cardNumber, String location, double amount, Date timestamp) {
        this.cardNumber = cardNumber;
        this.location = location;
        this.amount = amount;
        this.timestamp = timestamp;
    }
}

class FraudDetector {
    private static final double MAX_AMOUNT = 5000.0; // Threshold
    private static final int MAX_TRANSACTIONS = 3;
    private static final long TIME_FRAME_MS = 60000; // 1 minute

    // Store previous transactions for analysis
    private Map<String, List<Transaction>> transactionHistory = new HashMap<>();

    public boolean isFraudulent(Transaction transaction) {
        List<Transaction> history = transactionHistory.getOrDefault(transaction.cardNumber, new ArrayList<>());

        // Rule 1: Amount too high
        if (transaction.amount > MAX_AMOUNT) {
            System.out.println("⚠ High amount detected: $" + transaction.amount);
            return true;
        }

        // Rule 2: Too many transactions in short time
        long now = transaction.timestamp.getTime();
        long count = history.stream()
            .filter(t -> now - t.timestamp.getTime() <= TIME_FRAME_MS)
            .count();

        if (count >= MAX_TRANSACTIONS) {
            System.out.println("⚠ Multiple transactions in a short time");
            return true;
        }

        // Rule 3: Location mismatch (dummy rule here)
        if (!history.isEmpty() && !history.get(history.size() - 1).location.equals(transaction.location)) {
            System.out.println("⚠ Location mismatch detected");
            return true;
        }

        // Add transaction to history
        history.add(transaction);
        transactionHistory.put(transaction.cardNumber, history);
        return false;
    }
}

public class FraudDetectionDemo {
    public static void main(String[] args) throws InterruptedException {
        FraudDetector detector = new FraudDetector();
        String card = "1234-5678-9876-5432";

        // Simulate transactions
        Transaction t1 = new Transaction(card, "New York", 1200, new Date());
        Transaction t2 = new Transaction(card, "New York", 250, new Date());
        Thread.sleep(1000);
        Transaction t3 = new Transaction(card, "New York", 300, new Date());
        Thread.sleep(1000);
        Transaction t4 = new Transaction(card, "London", 150, new Date()); // Location mismatch

        List<Transaction> transactions = Arrays.asList(t1, t2, t3, t4);

        for (Transaction t : transactions) {
            boolean isFraud = detector.isFraudulent(t);
            System.out.println("Transaction of $" + t.amount + " from " + t.location + ": " + (isFraud ? "FRAUD" : "OK"));
        }
    }
}
