package com.electricitybill; 
 
import javax.swing.*; 
import java.awt.*; 
import java.awt.event.ActionEvent; 
import java.awt.event.ActionListener; 
import java.io.BufferedWriter; 
import java.io.FileWriter; 
import java.io.IOException; 
import java.nio.file.Files; 
import java.nio.file.Paths; 
import java.util.List; 
 
public class BillGUI extends JFrame { 
private JTextField customerNameField; 
private JTextField customerIDField; 
private JTextField unitsConsumedField; 
private JTextArea billArea; 
 
public BillGUI() { 
setTitle("Electricity Bill Management System"); 
setSize(400,400);   
setDefaultCloseOperation(JFrame.EXIT_ON_CLOS 
E); 
setLocationRelativeTo(null); 
setLayout(new BorderLayout()); 
 
JPanel inputPanel = new JPanel(new 
GridLayout(4,2)); 
inputPanel.add(new JLabel("Customer Name:")); 
customerNameField = new JTextField(); 
inputPanel.add(customerNameField); 
 
inputPanel.add(new JLabel("Customer ID:")); 
customerIDField = new JTextField(); 
inputPanel.add(customerIDField); 
 
inputPanel.add(new JLabel("Units Consumed:")); 
unitsConsumedField = new JTextField(); 
inputPanel.add(unitsConsumedField); 
 
JButton calculateButton = new JButton("Calculate 
Bill"); 
JButton saveButton = new JButton("Save Bill"); 
JButton loadButton = new JButton("Load Bills"); 
billArea = new JTextArea(); 
billArea.setEditable(false); 
 
calculateButton.addActionListener(new 
ActionListener() { 
@Override 
  
public void actionPerformed(ActionEvent e) { 
calculateBill(); 
 
} 
}); 
 
saveButton.addActionListener(new ActionListener() 
{ 
public void actionPerformed(ActionEvent e) { 
saveBill(); 
 
} 
}); 
 
loadButton.addActionListener(new ActionListener() 
{ 
public void actionPerformed(ActionEvent e) { 
loadBills(); 
} 
}); 
 
JPanel buttonPanel = new JPanel(); 
buttonPanel.add(calculateButton); 
buttonPanel.add(saveButton); 
buttonPanel.add(loadButton); 
 
 
add(inputPanel,BorderLayout.NORTH); 
  
add(new 
JScrollPane(billArea),BorderLayout.CENTER); 
add(buttonPanel,BorderLayout.SOUTH); 
 
} 
 
private void calculateBill() { 
String name = customerNameField.getText(); 
String id = customerIDField.getText(); 
int units = 
Integer.parseInt(unitsConsumedField.getText()); 
double ratePerUnit = 4.8; 
Bill bill = new Bill(name,id,units,ratePerUnit); 
billArea.setText(bill.getBillsDetails()); 
 
} 
private void saveBill() { 
try(BufferedWriter writer = new 
BufferedWriter(new FileWriter("bills.txt",true))) { 
writer.write(billArea.getText()); 
writer.write("\n\n"); 
JOptionPane.showMessageDialog(this,"Bill saved 
successfully!"); 
}catch(IOException e) { 
JOptionPane.showMessageDialog(this, "Error 
saving bill:" + e.getMessage()); 
 
} 
} 
 
private void loadBills() { 
try { 
List<String> lines = 
Files.readAllLines(Paths.get("bills.txt")); 
billArea.setText(""); 
for(String line : lines) { 
billArea.append(line + "\n"); 
} 
} catch(IOException e) { 
JOptionPane.showMessageDialog(this, "Error 
loading bills:" + e.getMessage()); 
 
} 
} 
 
 
 
public static void main(String[] args) { 
// TODO 
SwingUtilities.invokeLater(() -> { 
BillGUI gui = new BillGUI(); 
gui.setVisible(true); 
}); 
 
} 
 
} 
 
# BILL.JAVA 
 
package com.electricitybill; 
 
public class Bill { 
private String customerName; 
private String customerID; 
private int unitsConsumed; 
private double ratePerUnit; 
 
public Bill(String customerName,String 
customerID,int unitsConsumed,double ratePerUnit) 
{ 
this.customerName = customerName; 
this.customerID = customerID; 
this.unitsConsumed = unitsConsumed; 
this.ratePerUnit = ratePerUnit; 
 
} 
 
public double calculateBill() { 
return unitsConsumed * ratePerUnit; 
} 
 
public String getBillsDetails() { 
return "Electricity Bill\n" +"Customer Name:" 
+ customerName +"\n" +"Customer ID:" + customerID 
+"\n" +"Total Bill Amount:" + calculateBill();   
} 
public String getCustomerName() { 
return customerName; 
} 
public String getCustomerID() { 
return customerID; 
} 
} 
