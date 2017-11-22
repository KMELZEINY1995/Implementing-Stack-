# Implementing-Stack-
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class A3Elzeiny7823821 {

	public static void main(String[] args) {
		System.out.println("Assigment3  Winter 2017 COMP 2140");
		System.out.println("use stack to replace the postions with characters ");
		System.out.println("(Author [Khaled Mohamed Eleiny])\n");
		try {
			// read the integers from the file with the name given in args[0]
			BufferedReader input = new BufferedReader(new FileReader(new File(args[0])));
			int numsgrids = Integer.parseInt(input.readLine());
			int count = 0;
			String[][] matrix;
			while (count < numsgrids) {
				String[] line = (input.readLine()).split(" ");
				int m = Integer.parseInt(line[0]);
				int n = Integer.parseInt(line[1]);
				matrix = new String[m][n];

				for (int row = 0; row < matrix.length; row++) {
					String[] line2 = (input.readLine()).split("");
					for (int cols = 0; cols < matrix[0].length; cols++) {
						matrix[row][cols] = line2[cols];
					}
				}
				String[] alph = { "a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q",
						"r", "s", "t", "u", "v", "w", "x", "y", "z" };
				int countA = 0;
				Stack st = new Stack();
				Position newPos;
				while ((newPos = findPostion(matrix, st)) != null && count < 26) {
					newPos = findPostion(matrix, st);
					fillRegion(matrix, st, newPos, alph[countA]);
					countA++;
				}
				System.out.println("The number of grid is " + (count + 1) + "\n");
				print(matrix);

				count++;
			}
			input.close();
		} catch (IOException e) {
			System.out.println("file not FOUND");
		}

	}

	// this function get the positions from the matrix , push to the stack ,
	// then pop and check the positions
	public static void fillRegion(String matrix[][], Stack st, Position pos1, String alph) {
		if (pos1 != null) {
			matrix[pos1.getX()][pos1.getY()] = alph;
		}
		while (!st.isEmpty()) {

			Position pos2 = st.pop();
			if (pos2.getX() - 1 > 0 && matrix[pos2.getX() - 1][pos2.getY()].equals("0")) { 
				matrix[pos2.getX() - 1][pos2.getY()] = alph;
				Position pos3 = new Position(pos2.getX() - 1, pos2.getY());
				st.push(pos3);
			}
			if ((pos2.getX() + 1 < matrix.length) && matrix[pos2.getX() + 1][pos2.getY()].equals("0")) {
				matrix[pos2.getX() + 1][pos2.getY()] = alph;
				Position pos4 = new Position(pos2.getX() + 1, pos2.getY());
				st.push(pos4);
			}
			if (pos2.getY() - 1 > 0 && matrix[pos2.getX()][pos2.getY() - 1].equals("0")) {
				matrix[pos2.getX()][pos2.getY() - 1] = alph;
				Position pos5 = new Position(pos2.getX(), pos2.getY() - 1);
				st.push(pos5);
			}
			if ((pos2.getY() + 1 < matrix[0].length) && matrix[pos2.getX()][pos2.getY() + 1].equals("0")) {
				matrix[pos2.getX()][pos2.getY() + 1] = alph;
				Position pos6 = new Position(pos2.getX(), pos2.getY() + 1);
				st.push(pos6);
			}

		}

	}

	// this function find the position in the matrix and push it to the stack
	public static Position findPostion(String[][] matrix, Stack st) {
		boolean found = false;
		Position pos1 = null;
		for (int i = 0; !found && i < matrix.length; i++) {
			for (int j = 0; !found && j < matrix[0].length; j++) {
				if (matrix[i][j].equals("0")) {
					pos1 = new Position(i, j);
					st.push(pos1);
					found = true;
				}

			}
		}

		return pos1;
	}
	// print function 
	private static void print(String[][]matrix)
	  {
	    String result = "";
		for (int row = 0; row < matrix.length; row++) {
			for (int cols = 0; cols < matrix[0].length; cols++) {
				System.out.print(matrix[row][cols]);
			}
			System.out.print("\n");
		}
	    System.out.println(result);
	  }
}	

class Stack {
	private Node head; // the array of items

	public Stack() {
		head = null;
	}

	public boolean isEmpty() // check if the Stack is Empty
	{
		return (head == null);
	}

	public void push(Position one) // push the current item to add a new item
	{
		head = new Node(one, head);
	}

	public Position pop() {
		Position result;
		if (head != null) {
			result = head.getData();
			head = head.getNext();
		} else {
			System.out.println("Stack is empty");
			result = null;
		}
		return result;
	}

}

class Position {

	private int x;
	private int y;

	public Position(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public int getX() {
		return x;
	}

	public int getY() {
		return y;
	}

}

class Node {

	private Position data;
	private Node next;

	public Node(Position p, Node n) {
		data = p;
		next = n;
	}

	public Node getNext() {
		return next;
	}

	public void setNext(Node n) {
		next = n;
	}

	public Position getData() {
		return data;
	}

}
