import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.text.SimpleDateFormat;
import java.util.Date;

public class SupermarketUIProject {
    private static String[] items = {"Milk", "Bread", "Rice", "Eggs (per piece)", "Oil (1L)"};
    private static double[] prices = {40, 30, 60, 6, 150};
    private static int[] quantities = new int[items.length];
    private static double total = 0;
    private static String customerName = "";
    private static int billNo = (int)(Math.random() * 10000);

    public static void main(String[] args) {
        JFrame frame = new JFrame("Supermarket Billing System");
        frame.setSize(600, 500);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());

        JTextArea displayArea = new JTextArea();
        displayArea.setEditable(false);
        displayArea.setFont(new Font("Monospaced", Font.PLAIN, 14));
        JScrollPane scrollPane = new JScrollPane(displayArea);
        frame.add(scrollPane, BorderLayout.CENTER);

        JPanel bottomPanel = new JPanel();
        bottomPanel.setLayout(new FlowLayout());

        JButton showItemsBtn = new JButton("Show Items");
        JButton addToCartBtn = new JButton("Add to Cart");
        JButton viewCartBtn = new JButton("View Cart");
        JButton checkoutBtn = new JButton("Checkout");

        bottomPanel.add(showItemsBtn);
        bottomPanel.add(addToCartBtn);
        bottomPanel.add(viewCartBtn);
        bottomPanel.add(checkoutBtn);

        frame.add(bottomPanel, BorderLayout.SOUTH);

        customerName = JOptionPane.showInputDialog(frame, "Enter Customer Name:");

        showItemsBtn.addActionListener(e -> {
            displayArea.setText("Available Items:\n\n");
            for(int i = 0; i < items.length; i++) {
                displayArea.append((i+1) + ". " + items[i] + " - Rs." + prices[i] + "\n");
            }
        });

        addToCartBtn.addActionListener(e -> {
            String itemStr = JOptionPane.showInputDialog(frame, "Enter item number to buy:");
            String qtyStr = JOptionPane.showInputDialog(frame, "Enter quantity:");
            try {
                int itemNo = Integer.parseInt(itemStr) - 1;
                int qty = Integer.parseInt(qtyStr);
                if(itemNo >=0 && itemNo < items.length && qty > 0) {
                    quantities[itemNo] += qty;
                    JOptionPane.showMessageDialog(frame, qty + " " + items[itemNo] + " added to cart ✅");
                } else {
                    JOptionPane.showMessageDialog(frame, "Invalid input ❌");
                }
            } catch(Exception ex) {
                JOptionPane.showMessageDialog(frame, "Invalid input ❌");
            }
        });

        viewCartBtn.addActionListener(e -> {
            displayArea.setText("Items in Cart:\n\n");
            total = 0;
            boolean empty = true;
            for(int i=0; i<items.length; i++) {
                if(quantities[i]>0) {
                    double itemTotal = quantities[i]*prices[i];
                    total += itemTotal;
                    displayArea.append(items[i] + " x " + quantities[i] + " = Rs." + itemTotal + "\n");
                    empty = false;
                }
            }
            if(empty) displayArea.append("Cart is empty ❌\n");
            else displayArea.append("\nCurrent Total = Rs." + total + "\n");
        });

        checkoutBtn.addActionListener(e -> {
            displayArea.setText("===== FINAL BILL =====\n");
            displayArea.append("Bill No: " + billNo + "\n");
            displayArea.append("Customer: " + customerName + "\n");
            SimpleDateFormat formatter = new SimpleDateFormat("dd/MM/yyyy HH:mm:ss");
            displayArea.append("Date: " + formatter.format(new Date()) + "\n");
            displayArea.append("----------------------------\n");

            double finalTotal = 0;
            boolean empty = true;
            for(int i=0; i<items.length; i++) {
                if(quantities[i]>0) {
                    double itemTotal = quantities[i]*prices[i];
                    finalTotal += itemTotal;
                    displayArea.append(items[i] + " x " + quantities[i] + " = Rs." + itemTotal + "\n");
                    empty = false;
                }
            }
            if(empty) displayArea.append("Cart is empty ❌\n");
            else {
                double discount = (finalTotal > 1000) ? finalTotal * 0.10 : 0;
                double gst = (finalTotal - discount) * 0.05;
                double grandTotal = finalTotal - discount + gst;

                displayArea.append("----------------------------\n");
                displayArea.append("Total = Rs." + finalTotal + "\n");
                displayArea.append("Discount = Rs." + discount + "\n");
                displayArea.append("GST (5%) = Rs." + gst + "\n");
                displayArea.append("----------------------------\n");
                displayArea.append("Grand Total = Rs." + grandTotal + "\n");
                displayArea.append("============================\n");
                displayArea.append("   THANK YOU, VISIT AGAIN!  \n");
                displayArea.append("============================\n");
            }
        });

        frame.setVisible(true);
    }
}

