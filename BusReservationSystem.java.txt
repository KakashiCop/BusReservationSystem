import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;


public class BusReservationSystemGUI extends JFrame {
    private JButton[] seatButtons;
    private boolean[] seatAvailability;
    private JButton viewReservedSeatsButton;


    public BusReservationSystemGUI() {
        // Set up the frame
        setTitle("Bus Reservation System");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new GridLayout(20, 50, 100, 10));
        setResizable(false);


        // Initialize seat availability array
        seatAvailability = new boolean[35];
        


        // Initialize seat buttons
        viewReservedSeatsButton = new JButton("View Reserved Seats");
add(viewReservedSeatsButton);



        seatButtons = new JButton[35];
        for (int i = 0; i < 35; i++) {
            seatButtons[i] = new JButton("Seat " + (i + 1));
            add(seatButtons[i]);
            final int seatNumber = i + 1;
            seatButtons[i].addActionListener(new ActionListener() {
                @Override
                public void actionPerformed(ActionEvent e) {
                    if (seatAvailability[seatNumber - 1]) {
                        JOptionPane.showMessageDialog(null, "Seat " + seatNumber + " is already reserved.");
                    } else {
                        seatAvailability[seatNumber - 1] = true;
                        seatButtons[seatNumber - 1].setEnabled(false);
                        JOptionPane.showMessageDialog(null, "Seat " + seatNumber + " reserved successfully.");
                    }
                }
            });
        }
        viewReservedSeatsButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                StringBuilder reservedSeatsBuilder = new StringBuilder();
                reservedSeatsBuilder.append("Reserved Seats:\n");
                for (int i = 0; i < 35; i++) {
                    if (seatAvailability[i]) {
                        reservedSeatsBuilder.append("Seat ").append(i + 1).append("\n");
                    }
                }
                String reservedSeats = reservedSeatsBuilder.toString();
                JOptionPane.showMessageDialog(null, reservedSeats, "Reserved Seats", JOptionPane.INFORMATION_MESSAGE);
            }
        });
        setLayout(new GridLayout(3, 5, 10, 10));


        // Get user name and password
        String username = JOptionPane.showInputDialog(null, "Enter your username:");
        String password = JOptionPane.showInputDialog(null, "Enter your password:");


        // Print user name and password
        System.out.println("Username: " + username);
        System.out.println("Password: " + password);


        // Get the number of seats to be selected
        int numSeats = Integer.parseInt(JOptionPane.showInputDialog(null, "Enter the number of seats you want to select:"));


        // Make changes according to the number of seats
        if (numSeats < 1 || numSeats > 35) {
            JOptionPane.showMessageDialog(null, "Invalid number of seats!");
        } else {
            JOptionPane.showMessageDialog(null, "You are going to select " + numSeats + " seat(s).");
            int[] bookedSeats = new int[numSeats];
            int bookedCount = 0;


            for (int i = 0; i < 35 && bookedCount < numSeats; i++) {
                if (!seatAvailability[i]) {
                    seatAvailability[i] = true;
                    seatButtons[i].setEnabled(false);
                    bookedSeats[bookedCount] = i + 1;
                    bookedCount++;
                }
            }


            // Calculate the billing amount
            int basePrice = 10;
            int totalAmount = basePrice * numSeats;


            // Generate the bill
            StringBuilder billBuilder = new StringBuilder();
            billBuilder.append("========== Bus Reservation System ==========\n");
            billBuilder.append("Username: ").append(username).append("\n");
            billBuilder.append("Number of Seats: ").append(numSeats).append("\n");
            billBuilder.append("Booked Seats: ");
            for (int i = 0; i < numSeats; i++) {
                billBuilder.append(bookedSeats[i]);
                if (i < numSeats - 1) {
                    billBuilder.append(", ");
                }
            }
            billBuilder.append("\n");
            billBuilder.append("Total Amount: $").append(totalAmount).append("\n");
            billBuilder.append("===========================================\n");


            String bill = billBuilder.toString();
            JOptionPane.showMessageDialog(null, bill);
        }


        // Pack and display the frame
        pack();
        setLocationRelativeTo(null); // Center the frame on the screen
        setVisible(true);
    }


    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new BusReservationSystemGUI();
            }
        });
    }
}