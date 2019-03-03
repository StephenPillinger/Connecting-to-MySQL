# Connecting-to-MySQL
My failed attempts to connect to MySQL from Java

package model;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.util.Arrays;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;

import javax.sql.DataSource;

public class DataBase {

	private static final long serialVersionUID = 1L;
	
	LinkedList <Minion> db = new LinkedList <Minion> ();
	String[] stringForSerialisation;
	
	public void addNewEntry (Minion minion) {
		db.add(minion);
	}
	
	public Minion retrieveMinion(int index) {
		return db.get(index);
	}
	
	public void removeMinion (int index) {
		db.remove(index);
	}
	
	public List <Minion> getMinions () {
		return Collections.unmodifiableList(db);
	}
	
	public void connect () {
		
		//Going to use this to try to connect as John's methods aren't working
		//Try https://docs.oracle.com/javase/8/docs/api/java/sql/DriverManager.html
		
		//DataSource newObject;
		String url = "jdbc:mysql://localhost:3306/test";
		String user = "root";
		String password = "Password";
		
		try {
			//Class.forName("com.mysql.cj.jdbc.Driver");
			Connection con = DriverManager.getConnection(url, user, password);
			System.out.println(con);
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} /*catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} */
	}
	
	public void disconnect () {
		
	}
	
	public void saveToFile (File file) throws IOException {
		FileOutputStream fos = new FileOutputStream (file);
		ObjectOutputStream oos = new ObjectOutputStream (fos);
		Minion [] dataBaseAsArray = db.toArray(new Minion[db.size()]);
		oos.writeObject(dataBaseAsArray);
		int count = db.get(0).getCount();
		oos.writeInt(count);
		oos.close();
	}
	
	public void loadFromFile (File file) throws IOException {
		FileInputStream fis = new FileInputStream (file);
		ObjectInputStream ois = new ObjectInputStream (fis);
		
		try {
			Minion [] dataBaseAsArray = (Minion[]) ois.readObject();
			db.clear();
			db.addAll(Arrays.asList(dataBaseAsArray));
			int count = ois.readInt();
			db.get(0).setCount(count);
			ois.close();
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
}
