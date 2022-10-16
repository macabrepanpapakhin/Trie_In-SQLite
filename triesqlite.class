package hello;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Scanner;

public class triesqlite {
	static Connection conn=null;
	static int watcher=1;
	static String meaning="";
	static int showLimit=3;
	public static void main(String[] args) throws ClassNotFoundException, SQLException, IOException {
		Class.forName("org.sqlite.JDBC");
		conn=DriverManager.getConnection("jdbc:sqlite:testTrie.db");
		String tableName="test1";
		createTable(tableName);
		insertIntoTable(tableName,"*","-"," ");
	}

		addWord("Apple","the round fruit of a tree of the rose family, which typically has thin green or red skin and crisp flesh",tableName);
		addWord("Computer","an electronic device for storing and processing data, typically in binary form, according to instructions given to it in a variable program",tableName);
		addWord("App","In computering, an application, especially as downloaded by a user to a mobile device.\n" + 
				"",tableName);
		addWord("apply","make a formal application or request.",tableName);
		addWord("Compound","a thing that is composed of two or more separate elements; a mixture.",tableName);
		addWord("Composer","the state or feeling of being calm and in control of oneself.",tableName);
		addWord("Com","On the Internet, \"com\" is one of the top-level domain names that can be used when choosing a domain name. It generally describes the entity owning the domain name as a commercial organization.",tableName);
		display(tableName);
		
		 	
		while(true) {
			Scanner sc=new Scanner(System.in);
			System.out.println("enter word to find");
			String s1=sc.nextLine();
			if(searchWord(s1,tableName)) {
				
				System.out.println("found the meaning :: = ");
			}else {
				System.out.println("cannot find the word :: => "+"'-'");
			}
				System.out.println("recommend words "+wordsToRecommend);
			}
	

			
	}
	static String prefix="";
	private static boolean searchWord(String word,String tableName) throws SQLException {
		prefix+=word;
		ResultSet result=retrieveByIndex(1,tableName);
		String references=result.getString("referencesNext");
		watcher=1;
		for(int j=0;j<word.length();j++) {
			char ch=word.charAt(j);
			ResultSet res=retrieveByIndex((watcher),tableName);
			references=res.getString("referencesNext");
			
			if(!containsKey(references,ch+"")) {
				//System.out.println("watcher is now ::=>"+watcher);
				return false;
				
			}

			
		}
		
		ResultSet rs=retrieveByIndex(watcher,tableName);
		meaning=rs.getString("meaningIndex");
		count=0;
		wordsToRecommend="";
		generateRecommend(watcher,"",tableName);
		if(rs.getString("meaningIndex").contentEquals("-")) {
			return false;
		}else {
			System.out.println(rs.getString("meaningIndex"));
		}
	
		return true;
		
	}
	static int count=0;
	static String wordsToRecommend="";
	private static void generateRecommend(int id,String currentWord,String tableName) throws SQLException {
		
		ResultSet rs=retrieveByIndex(id,tableName);
		String currentReferences=rs.getString("referencesNext");
		if(currentReferences==null || currentReferences.length()<=1) {
			System.out.println("Dead References : continue:::");
			String hasMeaning=rs.getString("meaningIndex");
			if(!hasMeaning.contentEquals("-")) {
				wordsToRecommend+=("#"+(prefix+currentWord));
			}
			return;
		}
	
		String[] refs=currentReferences.split("#");
		
		for(String col:refs) {
			if(col.length()<=1)continue;
			//System.out.println("val is  "+col);
			String[] key=col.split("/");
			if(key.length==1) {
				//System.out.println("error is "+key[0]);
				return;
			}
			String ch=key[0];
			int index=Integer.parseInt(key[1]);
			generateRecommend(index,currentWord+ch,tableName);
		}
		
	}
	
	private static boolean containsKey(String references,String ch) {
		
		    if(references==null) {
		    	System.out.println("what??");
		    	return false;
		    }
		  
			String[] refs=references.split("#");
			for(String col:refs) {
				String[] key=col.split("/");
				if(key.length!=2)continue;
				if(key[0].contentEquals(ch)) {
					watcher=Integer.parseInt(key[1]);
					//System.out.println("watcher is now containes:=>"+watcher);
					
					return true;
				}
			}
			return false;
		
	}
	
	private static void addWord(String word,String meaning,String tableName) throws SQLException {
		ResultSet result=retrieveByIndex(1,tableName);
		String references=result.getString("referencesNext");
		watcher=1;
		//System.out.println("word length is "+word.length());
		for(int j=0;j<word.length();j++) {
			char ch=word.charAt(j);
			ResultSet res=retrieveByIndex((watcher),tableName);
			references=res.getString("referencesNext");
		if(containsKey(references,ch+"")) {
				//System.out.println("chainging contains :=> "+references);
			}else {
				insertIntoTable(tableName,ch+"","-","");
				ResultSet ss=getLastRow(tableName);
				int newIndex=ss.getInt(1);
				String newData=references+"#"+(ch+"/"+newIndex);
				//System.out.println("watcher before update"+watcher);
				updateReferencesByIndex(watcher,newData,tableName);
				watcher=newIndex;
				//System.out.println("watcher is now ::=>"+watcher);
			
			}

			
		}
		updateMeaningByIndex(watcher,meaning,tableName);
		
	}
	private static void updateMeaningByIndex(int watcher,String meaning,String tableName) throws SQLException {
		String sql="UPDATE "+ tableName+ " \n" + 
				"SET meaningIndex = ?"+
				"WHERE id = "+watcher+ ";";
		PreparedStatement statement = conn.prepareStatement(sql);
		statement.setString(1,meaning);
		statement.executeUpdate();
	}
	private static ResultSet getLastRow(String tableName) throws SQLException{
		String sql="SELECT * FROM "+tableName +" ORDER BY id DESC LIMIT 1;";
		Statement stmt=conn.createStatement();
		ResultSet result=stmt.executeQuery(sql);
		return result;
		
		
	}
	private static int largestIndex(String tableName) throws SQLException {
		String sql="SELECT * FROM "+tableName +" ORDER BY id DESC LIMIT 1;";
		Statement stmt=conn.createStatement();
		ResultSet result=stmt.executeQuery(sql);
		return result.getInt(1);
	}
	private static void updateReferencesByIndex(int id,String newData,String tableName) throws SQLException{
		String sql="UPDATE "+ tableName+ " \n" + 
				"SET referencesNext = ?"+
				"WHERE id = "+id+ ";";
		PreparedStatement statement = conn.prepareStatement(sql);
		statement.setString(1,newData);
		statement.executeUpdate();
	}
	private static ResultSet retrieveByIndex(int index,String tableName) throws SQLException {
		String sql="SELECT * FROM "+tableName+ " WHERE rowid = "+index;
		Statement stmt=conn.createStatement();
		ResultSet rs=stmt.executeQuery(sql);
		return rs;
		//will run in log(n) time according to sqlite documentation
	}
	
	
	private static void createTable(String tb) throws SQLException {
		String sqlquery="CREATE TABLE if not exists " +tb+"(\n" + 
				"   id INTEGER PRIMARY KEY ,\n" +
				"   hasMeaning TEXT ,\n" + 
				"   meaningIndex TEXT ,\n" + 
				"   referencesNext TEXT \n" + 
				");";
		Statement stmt=conn.createStatement();	
		stmt.execute(sqlquery);
	}
	private static void insertIntoTable(String tb,String a,String b,String c) throws SQLException {
		String insertSQL="insert into "+tb+" (hasMeaning,meaningIndex,referencesNext) values (?,?,?)";
		PreparedStatement ptst=conn.prepareStatement(insertSQL);
		ptst.setString(1, a);
		ptst.setString(2, b);
		ptst.setString(3, c);
		ptst.executeUpdate();
	}
	private static void display(String tableName) throws SQLException {
		System.out.println("Showing data below.......");
		String sql="select * from "+tableName;
		Statement stmt=conn.createStatement();
		ResultSet rs=null;
		try{
			rs=stmt.executeQuery(sql);
			}
		catch(Exception e) {
			System.out.println("no such table exists");
		}
		while(rs.next()) {
		
			System.out.println(rs.getString("hasMeaning")+" "+rs.getString("meaningIndex")+rs.getString("referencesNext"));
		}
	}
	

}
