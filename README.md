import javax.swing.*; 
import java.awt.*; 
import java.awt.event.ActionEvent; 
import java.awt.event.ActionListener; 
import java.util.ArrayList; 
import java.util.Enumeration; 
 
class Question { 
    String question; 
    ArrayList<String> options; 
    int correctOption; 
 
    public Question(String question, ArrayList<String> options, int correctOption) { 
        this.question = question; 
        this.options = options; 
        this.correctOption = correctOption; // Zero-based index for the correct option
    } 
} 
 
public class QuizGUI extends JFrame { 
    private ArrayList<Question> questions = new ArrayList<>(); 
    private int score = 0; 
    private int questionIndex = 0; 
 
    private JLabel questionLabel; 
    private ButtonGroup optionGroup; 
    private JButton submitButton; 
 
    public QuizGUI() { 
        // Sample questions
        questions.add(new Question("What is the capital of France?", 
                new ArrayList<>(java.util.List.of("A. London", "B. Berlin", "C. Paris", "D. Madrid")), 2)); // Correct option is 2 (zero-based)
 
        questions.add(new Question("Which programming language is this quiz written in?", 
                new ArrayList<>(java.util.List.of("A. Python", "B. Java", "C. JavaScript", "D. C++")), 1)); // Correct option is 1 (zero-based)
 
        questions.add(new Question("What is the largest mammal?", 
                new ArrayList<>(java.util.List.of("A. Elephant", "B. Blue Whale", "C. Giraffe", "D. Gorilla")), 1)); // Correct option is 1 (zero-based)
 
        questions.add(new Question("What is the capital of Japan?", 
                new ArrayList<>(java.util.List.of("A. Seoul", "B. Beijing", "C. Tokyo", "D. Bangkok")), 2)); // Correct option is 2 (zero-based)
 
        // Set up JFrame 
        setTitle("Online Quiz System"); 
        setSize(400, 300); 
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); 
 
        // Set up components 
        questionLabel = new JLabel(); 
        optionGroup = new ButtonGroup(); 
 
        JPanel optionsPanel = new JPanel(); 
        optionsPanel.setLayout(new GridLayout(0, 1)); 
 
        for (int i = 0; i < 4; i++) { 
            JRadioButton optionButton = new JRadioButton(); 
            optionButton.setActionCommand(String.valueOf(i + 1)); 
            optionGroup.add(optionButton); 
            optionsPanel.add(optionButton); 
        } 
 
        submitButton = new JButton("Submit"); 
        submitButton.addActionListener(new ActionListener() { 
            @Override 
            public void actionPerformed(ActionEvent e) { 
                if (optionGroup.getSelection() != null) { // Check if an option is selected
                    processAnswer();
                    questionIndex++; // Move to the next question
                    displayQuestion();
                } else {
                    JOptionPane.showMessageDialog(null, "Please select an option before submitting.");
                }
            } 
        }); 
 
        // Set up layout 
        setLayout(new BorderLayout()); 
        add(questionLabel, BorderLayout.NORTH); 
        add(optionsPanel, BorderLayout.CENTER); 
        add(submitButton, BorderLayout.SOUTH); 
 
        // Display the first question 
        displayQuestion(); 
    } 
 
    private void displayQuestion() { 
        optionGroup.clearSelection(); // Clear previous selection
        if (questionIndex < questions.size()) { 
            Question currentQuestion = questions.get(questionIndex); 
            questionLabel.setText(currentQuestion.question); 
             
            Enumeration<AbstractButton> buttons = optionGroup.getElements(); 
            int optionIndex = 0; 
            while (buttons.hasMoreElements()) { 
                AbstractButton button = buttons.nextElement(); 
                button.setText(currentQuestion.options.get(optionIndex)); 
                optionIndex++; 
            } 
        } else { 
            showResult(); 
        } 
    } 
 
    private void processAnswer() { 
        ButtonModel selectedButton = optionGroup.getSelection(); 
 
        if (selectedButton != null) { 
            int selectedOption = Integer.parseInt(selectedButton.getActionCommand()); 
            Question currentQuestion = questions.get(questionIndex); 
 
            if (selectedOption == currentQuestion.correctOption + 1) { // Adjusted for 1-based index
                score++; 
            } 
        } 
    } 


 
    private void showResult() { 
        JOptionPane.showMessageDialog(this, "Quiz completed. Your final score: " + score + "/" + questions.size()); 
        System.exit(0); 
    } 


 
    public static void main(String[] args) { 
        SwingUtilities.invokeLater(() -> { 
            new QuizGUI().setVisible(true); 
        }); 
    } 
}
