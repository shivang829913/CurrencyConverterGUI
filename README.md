package niet.in;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.HashMap;
import java.awt.Window.Type;

// Custom Exception
class InvalidCurrencyException extends Exception {
    public InvalidCurrencyException(String message) {
        super(message);
    }
}

// Converter Logic Class
class Converter {

    private static HashMap<String, Double> rates = new HashMap<>();

    static {
        rates.put("USD", 83.50);
        rates.put("INR", 1.0);
        rates.put("EUR", 90.10);
        rates.put("JPY", 0.56);
    }

    public static double convert(double amount, String from, String to) throws InvalidCurrencyException {
        if (!rates.containsKey(from) || !rates.containsKey(to))
            throw new InvalidCurrencyException("❌ Invalid currency code!");

        double amountInINR = amount * rates.get(from);
        return amountInINR / rates.get(to);
    }
}

// GUI Class (Main)
public class CurrencyConverterGUI1 extends JFrame implements ActionListener {

    JTextField amountField, fromField, toField;
    JLabel resultLabel;
    JButton convertButton;

    public CurrencyConverterGUI1() {
    	setType(Type.POPUP);
    	getContentPane().setBackground(new Color(255, 255, 0));
        setTitle("Currency Converter");
        setSize(522, 355);
        getContentPane().setLayout(null);

        JLabel lblAmount = new JLabel("Amount:");
        lblAmount.setForeground(new Color(0, 0, 0));
        lblAmount.setFont(new Font("Algerian", Font.ITALIC, 15));
        lblAmount.setBounds(99, 23, 90, 54);
        getContentPane().add(lblAmount);
        amountField = new JTextField();
        amountField.setBounds(266, 32, 180, 38);
        getContentPane().add(amountField);

        JLabel label_1 = new JLabel("From Currency (USD/INR/EUR/JPY):");
        label_1.setFont(new Font("Algerian", Font.ITALIC, 12));
        label_1.setBounds(36, 78, 220, 38);
        getContentPane().add(label_1);
        fromField = new JTextField();
        fromField.setBounds(266, 78, 180, 38);
        getContentPane().add(fromField);

        JLabel label_2 = new JLabel("To Currency (USD/INR/EUR/JPY):");
        label_2.setFont(new Font("Algerian", Font.ITALIC, 12));
        label_2.setBounds(36, 126, 210, 38);
        getContentPane().add(label_2);
        toField = new JTextField();
        toField.setBounds(266, 126, 180, 38);
        getContentPane().add(toField);

        convertButton = new JButton("Convert");
        convertButton.setBackground(new Color(0, 255, 0));
        convertButton.setFont(new Font("Algerian", Font.PLAIN, 10));
        convertButton.setBounds(153, 206, 180, 38);
        convertButton.addActionListener(this);
        getContentPane().add(convertButton);

        resultLabel = new JLabel("Result: ");
        resultLabel.setFont(new Font("Algerian", Font.PLAIN, 15));
        resultLabel.setBounds(131, 270, 265, 38);
        getContentPane().add(resultLabel);

        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);
        setVisible(true);
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        try {
            double amount = Double.parseDouble(amountField.getText());
            String from = fromField.getText().toUpperCase();
            String to = toField.getText().toUpperCase();

            double result = Converter.convert(amount, from, to);
            resultLabel.setText("Result: " + amount + " " + from + " = " + result + " " + to);

        } catch (NumberFormatException ex) {
            resultLabel.setText("❌ Invalid amount format!");
        } catch (InvalidCurrencyException ex) {
            resultLabel.setText("❌ " + ex.getMessage());
        } catch (Exception ex) {
            resultLabel.setText("❌ Error in conversion!");
        }
    }

    public static void main(String[] args) {
        new CurrencyConverterGUI1();
    }
}


