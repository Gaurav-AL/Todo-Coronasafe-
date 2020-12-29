# Todo-Coronasafe-
import java.util.*;
import java.io.BufferedReader; 
import java.io.IOException; 
import java.io.*;
import java.nio.file.Files;
import java.time.format.DateTimeFormatter;  
import java.time.LocalDateTime;  
import java.io.InputStreamReader; 
public class Todo {
	private static final String FILE_PATH=".dataFileMyProgram";
	private static final String COMPLETED_FILE_PATH=".dataFileMyProgramCompleted";
	private static File dataFile = new File(FILE_PATH);
	private static File completedDataFile = new File(COMPLETED_FILE_PATH);
	public Todo() throws IOException{
		dataFile.createNewFile();
		completedDataFile.createNewFile();
	}

	public static List<String> readFileToList(String filePath) throws FileNotFoundException,IOException{
		BufferedReader in = new BufferedReader(new FileReader(filePath));
		String str;
		List<String> list = new ArrayList<String>();
		while((str = in.readLine()) != null){
		    list.add(str);
		}
		return list;
	}

	public static void writeToFile(File inputfile,String line,Boolean append) throws IOException{
		FileWriter file= new FileWriter(inputfile, append);
		file.write(line);
		file.close();
	}

	public static void writeToFileOnNewLine(File inputFile,String line,Boolean append) throws IOException{
		writeToFile(inputFile,line+"\n",append);
	}

	public static void writeListToFile(File inputFile,List<String> data) throws IOException{
		Boolean firstLine=true;
		for(String items:data){
			if(firstLine){
				writeToFileOnNewLine(inputFile,items,false);
				firstLine=false;
			}
			else
				writeToFileOnNewLine(inputFile,items,true);
		}
	}



	public static void main(String args[])throws IOException{
		Todo todo= new Todo();
		//List<String> todo = new ArrayList<>();
		BufferedReader reader =  
                   new BufferedReader(new InputStreamReader(System.in)); 
         Boolean exit=false; 
		if(args[0].equals("help")){
		System.out.println("Usage :-");
		System.out.println("Java Todo add "+        "         # add todo item");
		System.out.println("Java Todo ls "+         "         # Show remaining Todos");
		System.out.println("Java Todo del NUMBER"+  "         # Delete a todo");
		System.out.println("Java Todo done NUMBER"+ "         # Complete a todo");
		System.out.println("Java Todo help "+       "         # show Usage");
		System.out.println("Java Todo report "+     "         # Statictics");
		}
		if(args[0].equals("ls")){
			List<String> data=readFileToList(FILE_PATH);
			int counter=1;
			for(String listItem:data){
				System.out.println("["+counter+"] "+listItem);
				counter++;
			}
		}
		if(args[0].equals("add")){
			//todo.add(input.replaceFirst(splitted[0],""));
			writeToFileOnNewLine(dataFile,args[1],true);
			System.out.println("Added item :"+args[1]);
			
		}
		if(args[0].equals("del")){
			List<String> data=readFileToList(FILE_PATH);
			data.remove(Integer.parseInt(args[1])-1);
			writeListToFile(dataFile,data);
		}
		if(args[0].equals("done")){
			List<String> data=readFileToList(FILE_PATH);
			List<String> doneData=readFileToList(COMPLETED_FILE_PATH);
			doneData.add(data.get(Integer.parseInt(args[1])-1));
			data.remove(Integer.parseInt(args[1])-1);
			writeListToFile(dataFile,data);
			writeListToFile(completedDataFile,doneData);
			System.out.println("Done: Pending: "+readFileToList(FILE_PATH).size()+", Completed: "+readFileToList(COMPLETED_FILE_PATH).size());
		}
		if(args[0].equals("report")){
			DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy/MM/dd");  
   			LocalDateTime now = LocalDateTime.now();  
   			System.out.println(dtf.format(now)+" Pending: "+readFileToList(FILE_PATH).size()+", Completed: "+readFileToList(COMPLETED_FILE_PATH).size());
		}
	}
}
#Done.
