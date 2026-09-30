# java-email-categorizer
java-email-categorizer....
import java.util.*;

public class EmailCategorizer {

    static Map<String, List<String>> categoryKeywords = new LinkedHashMap<>();

    static void initCategories() {
        categoryKeywords.put("Work", Arrays.asList("meeting", "project", "deadline", "report", "client", "invoice"));
        categoryKeywords.put("Academic", Arrays.asList("exam", "assignment", "result", "lecture", "syllabus", "university"));
        categoryKeywords.put("Promotions", Arrays.asList("sale", "offer", "discount", "deal", "off", "coupon"));
        categoryKeywords.put("Social", Arrays.asList("birthday", "party", "invite", "wedding", "friend"));
        categoryKeywords.put("Spam", Arrays.asList("free money", "lottery", "click here", "winner", "urgent reply"));
    }

    static String classify(String subject, String body) {
        String text = (subject + " " + body).toLowerCase();
        String bestCategory = "General";
        int bestScore = 0;

        for (Map.Entry<String, List<String>> entry : categoryKeywords.entrySet()) {
            int score = 0;
            for (String keyword : entry.getValue()) {
                if (text.contains(keyword)) score++;
            }
            if (score > bestScore) {
                bestScore = score;
                bestCategory = entry.getKey();
            }
        }
        return bestCategory;
    }

    static class Email {
        String subject, body;
        Email(String subject, String body) {
            this.subject = subject;
            this.body = body;
        }
    }

    public static void main(String[] args) {
        initCategories();

        List<Email> inbox = new ArrayList<>();
        inbox.add(new Email("Project deadline reminder", "Please submit the client report by Friday"));
        inbox.add(new Email("Mid-sem exam schedule", "Your university exam timetable has been released"));
        inbox.add(new Email("50% off on electronics", "Grab this amazing discount deal today"));
        inbox.add(new Email("You are a lottery winner!", "Click here to claim your free money now"));
        inbox.add(new Email("Birthday party invite", "You're invited to Riya's birthday celebration"));

        System.out.println("=== Email Categorization Report ===\n");
        for (Email e : inbox) {
            System.out.printf("Subject : %s%nCategory: %s%n%n", e.subject, classify(e.subject, e.body));
        }
    }
}
