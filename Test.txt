import java.io.*;

public class TestScaleApps {
    public static void main(String[] args) {

        //проверка на наличие 2-ух аргументов
        if (args.length != 2) {
            System.out.println("Enter two arguments!");
            return;
        }

        //PART 1
        //PART 1.1 если первый аргумент "-", используем для ввода консоль
        if ("-".equals(args[0])) {

            BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

            //если второй аргумент "-", используем для вывода консоль
            if ("-".equals(args[1])) {

                System.out.println("Enter an arithmetic action and numbers separated by spaces:" + "\n" +
                        "add - addition of two or more numbers, " + "\n" +
                        "mul - multiplication of two or more numbers, " + "\n" +
                        "ex - multiplying the first two numbers and adding with the third number. " + "\n" +
                        "To cancel enter the \"exit\".");

                try {
                    boolean correctFormat = false;
                    while (!correctFormat) { //считываем строку пока вводится неверный формат строки

                        String readString = bufferedReader.readLine();

                        if (readString.equals("exit")) break; //если ввести exit то программа закроется

                        String[] parts = new String[] {"null"};

                        //если строка не пустая или не состоит из пробелов, то
                        if (!isBlank(readString)){
                            parts = readString.split(" "); //делим строку по пробелам и добавляем в массив
                        }

                        //Проверяем на корректность введенную строку. Строка корректная, если:
                        //первое слово add, mul или ex
                        //введено больше или равно трех слов
                        //начиная со второго слова введены числа
                        if ((parts[0].equals("add") || parts[0].equals("mul") || parts[0].equals("ex")) && parts.length >= 3) {
                            boolean allNum = true;
                            for (int i = 1; i < parts.length; ) {
                                if (isNumeric(parts[i])) {
                                    i++;
                                } else {
                                    System.out.println("Wrong format! To cancel enter the \"exit\".");
                                    allNum = false;
                                    break;
                                }
                            }
                            if (allNum) {
                                if (parts[0].equals("add")) {
                                    System.out.println("Result: " + addition(parts));
                                    correctFormat = true;
                                } else if (parts[0].equals("mul")) {
                                    System.out.println("Result: " + multiplication(parts));
                                    correctFormat = true;
                                } else if (parts[0].equals("ex") && parts.length == 4) {
                                    System.out.println("Result: " + arithmeticExample(parts));
                                    correctFormat = true;
                                } else System.out.println("Wrong format! The \"ex\"-function requires 3 numbers! " +
                                        "\n" + "To cancel enter the \"exit\".");
                            }
                        } else System.out.println("Wrong format! To cancel enter the \"exit\".");
                    }
                    bufferedReader.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            //PART 1.2 если второй аргумент не "-", используем для вывода файл
            else {
                //проверка, что второй аргумент - корректное имя текстового файла
                String[] secondArgumentParts = args[1].split("\\.");
                if (secondArgumentParts.length == 2 && secondArgumentParts[1].equals("txt") && !isIncorrectName(secondArgumentParts[0])) {
                    try {
                        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(args[1]));

                        System.out.println("Enter an arithmetic action and numbers separated by spaces:" + "\n" +
                                "add - addition of two or more numbers, " + "\n" +
                                "mul - multiplication of two or more numbers, " + "\n" +
                                "ex - multiplying the first two numbers and adding with the third number. " + "\n" +
                                "To cancel enter the \"exit\".");

                        boolean correctFormat = false;
                        while (!correctFormat) { //считываем строку, пока вводится неверный формат примера

                            String readString = bufferedReader.readLine();

                            if (readString.equals("exit")) break; //если ввести exit то программа закроется

                            String[] parts = new String[] {"null"};

                            if (!isBlank(readString)){
                                parts = readString.split(" ");
                            } //делим строку по пробелам и добавляем в массив

                            //Проверяем на корректность введенную строку. Строка корректная, если:
                            //первое слово add, mul или ex
                            //введено больше или равно трех слов
                            //начиная со второго слова введены числа
                            if ((parts[0].equals("add") || parts[0].equals("mul") || parts[0].equals("ex")) && parts.length >= 3) {
                                boolean allNum = true;
                                for (int i = 1; i < parts.length; ) {
                                    if (isNumeric(parts[i])) {
                                        i++;
                                    } else {
                                        System.out.println("Wrong format! To cancel enter the \"exit\".");
                                        allNum = false;
                                        break;
                                    }
                                }
                                if (allNum) {
                                    if (parts[0].equals("add")) {
                                        bufferedWriter.write("Result: " + addition(parts));
                                        correctFormat = true;
                                    } else if (parts[0].equals("mul")) {
                                        bufferedWriter.write("Result: " + multiplication(parts));
                                        correctFormat = true;
                                    } else if (parts[0].equals("ex") && parts.length == 4) {
                                        bufferedWriter.write("Result: " + arithmeticExample(parts));
                                        correctFormat = true;
                                    } else System.out.println("Wrong format! The \"ex\"-function requires 3 numbers! " +
                                            "\n" + "To cancel enter the \"exit\".");
                                }
                            } else System.out.println("Wrong format! To cancel enter the \"exit\".");
                        }
                        bufferedReader.close();
                        bufferedWriter.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                } else {
                    System.out.println("Enter correct arguments!");
                }
            }
        }

        //PART 2
        //если первый аргумент не "-", используем для ввода файл
        else {
            //проверка, что первый аргумент - корректное имя текстового файла
            String[] firstArgumentParts = args[0].split("\\.");
            if (firstArgumentParts.length == 2 && firstArgumentParts[1].equals("txt") && !isIncorrectName(firstArgumentParts[0])) {
                try {
                    BufferedReader bufferedReader = new BufferedReader(new FileReader(args[0]));

                    //PART 2.1 если второй аргумент "-", используем для вывода консоль
                    if ("-".equals(args[1])) {
                        try {
                            String stringFile = bufferedReader.readLine();

                            String[] parts = new String[] {"null"};

                            if (!isBlank(stringFile)){
                                parts = stringFile.split(" "); //делим строку по пробелам и добавляем в массив
                            }

                            //Проверяем на корректность введенную строку. Строка корректная, если:
                            //первое слово add, mul или ex
                            //введено больше или равно трех слов
                            //начиная со второго слова введены числа
                            if ((parts[0].equals("add") || parts[0].equals("mul") || parts[0].equals("ex")) && parts.length >= 3) {
                                boolean allNum = true;
                                for (int i = 1; i < parts.length; ) {
                                    if (isNumeric(parts[i])) {
                                        i++;
                                    } else {
                                        System.out.println("Wrong format! Please, check data format in the file.");
                                        allNum = false;
                                        break;
                                    }
                                }
                                if (allNum) {
                                    if (parts[0].equals("add")) {
                                        System.out.println("Result: " + addition(parts));

                                    } else if (parts[0].equals("mul")) {
                                        System.out.println("Result: " + multiplication(parts));

                                    } else if (parts[0].equals("ex") && parts.length == 4) {
                                        System.out.println("Result: " + arithmeticExample(parts));

                                    } else System.out.println("Wrong format! The \"ex\"-function requires 3 numbers! " +
                                            "\n" + "Please, check data format in the file.");
                                }
                            } else System.out.println("Wrong format! Please, check data format in the file.");
                            bufferedReader.close();
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                    //PART 2.2 если второй аргумент не "-", используем для вывода файл
                    else {
                        //проверка, что второй аргумент - корректное имя текстового файла
                        String[] secondArgumentParts = args[1].split("\\.");
                        if (secondArgumentParts.length == 2 && secondArgumentParts[1].equals("txt") && !isIncorrectName(secondArgumentParts[0])) {
                            try {
                                BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(args[1]));

                                String stringFile = bufferedReader.readLine();

                                String[] parts = new String[] {"null"};

                                if (!isBlank(stringFile)){
                                    parts = stringFile.split(" ");
                                }

                                //Проверяем на корректность введенную строку. Строка корректная, если:
                                //первое слово add, mul или ex
                                //введено больше или равно трех слов
                                //начиная со второго слова введены числа
                                if ((parts[0].equals("add") || parts[0].equals("mul") || parts[0].equals("ex")) && parts.length >= 3) {
                                    boolean allNum = true;
                                    for (int i = 1; i < parts.length; ) {
                                        if (isNumeric(parts[i])) {
                                            i++;
                                        } else {
                                            System.out.println("Wrong format! Please, check data format in the file.");
                                            allNum = false;
                                            break;
                                        }
                                    }
                                    if (allNum) {
                                        if (parts[0].equals("add")) {
                                            bufferedWriter.write("Result: " + addition(parts));
                                        } else if (parts[0].equals("mul")) {
                                            bufferedWriter.write("Result: " + multiplication(parts));
                                        } else if (parts[0].equals("ex") && parts.length == 4) {
                                            bufferedWriter.write("Result: " + arithmeticExample(parts));
                                        } else
                                            System.out.println("Wrong format! The \"ex\"-function requires 3 numbers! " +
                                                    "\n" + "Please, check data format in the file.");
                                    }
                                } else System.out.println("Wrong format! Please, check data format in the file.");

                                bufferedReader.close();
                                bufferedWriter.close();
                            } catch (IOException e) {
                                e.printStackTrace();
                            }
                        } else {
                            System.out.println("Enter correct arguments!");
                        }
                    }
                } catch (FileNotFoundException fnfe) {
                    System.out.println("Read file not found! " + "\n" +
                            "If you want to use the console, enter a character \"-\" as an argument.");
                }
            } else {
                System.out.println("Enter correct arguments!");
            }
        }
    }

    private static double addition(String[] task) {
        double sum = 0;
        for (int i = 1; i < task.length; i++) {
            try {
                sum += Double.parseDouble(task[i]);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return sum;
    }

    private static double multiplication(String[] task) {
        double prod = 1;
        for (int i = 1; i < task.length; i++) {
            try {
                prod *= Double.parseDouble(task[i]);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return prod;
    }

    private static double arithmeticExample(String[] task) {
        double result;
        double firstNum = Double.parseDouble(task[1]);
        double secondNum = Double.parseDouble(task[2]);
        double thirdNum = Double.parseDouble(task[3]);
        result = firstNum * secondNum + thirdNum;
        return result;
    }

    private static boolean isNumeric(String str) {
        try {
            double d = Double.parseDouble(str);
        } catch (NumberFormatException nfe) {
            return false;
        }
        return true;
    }

    private static boolean isBlank(final CharSequence cs) {
        int strLen;
        if (cs == null || (strLen = cs.length()) == 0) {
            return true;
        }
        for (int i = 0; i < strLen; i++) {
            if (!Character.isWhitespace(cs.charAt(i))) {
                return false;
            }
        }
        return true;
    }

    private static boolean isIncorrectName (String string) {
        if (string == null || string.length() == 0) {
            return true;
        }
        for (int i = 0; i < string.length(); i++) {
            if (string.contains("\\") || string.contains("/") || string.contains(":") || string.contains("*") || string.contains("?") || string.contains("\"") || string.contains("<") || string.contains(">") || string.contains("|")) {
                return true;
            }
        }
        return false;
    }
}

