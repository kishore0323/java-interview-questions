```
package interview.program.stream;

import java.time.YearMonth;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.HashSet;
import java.util.LinkedHashMap;
import java.util.LinkedHashSet;
import java.util.List;
import java.util.Map;
import java.util.Objects;
import java.util.Optional;
import java.util.Random;
import java.util.Set;
import java.util.function.BiFunction;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class TopStream {

	// Basic Math Programs
	// 1. Positive or Negative Numbers
	public String positiveOrNegative(int num) {
		return num > 0 ? "Positive" : num < 0 ? "Negative" : "Zero";
	}
	
	// 2. Even or Odd Numbers
	public String evenOrOdd(int num) {
		return num % 2 == 0 ? "Even" : "Odd";
	}

	// 3. Sum of First N Natural Numbers
	public int sumFirstNNumbers(int n) {
		return n * (n + 1) / 2;
	}

	// 5. Sum of Numbers in a Given Range
	public int sumInRange(int start, int end) {
		return (end * (end + 1) / 2) - ((start - 1) * start / 2);
	}

	// 6. Greatest of Two Numbers
	public int greatestOfTwo(int a, int b) {
		return Math.max(a, b);
	}

	// 7. Greatest of Three Numbers
	public int greatestOfThree(int a, int b, int c) {
		return Math.max(a, Math.max(b, c));
	}

	// 8. Leap Year or Not
	public boolean isLeapYear(int year) {
		return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
	}

	// 9. Prime Number
	public boolean isPrime(int num) {
		if (num <= 1)
			return false;
		for (int i = 2; i <= Math.sqrt(num); i++) {
			if (num % i == 0)
				return false;
		}
		return true;
	}

	// 10. Prime Number within a Given Range
	public List<Integer> primesInRange(int start, int end) {
		return IntStream.rangeClosed(start, end).filter(this::isPrime).boxed().collect(Collectors.toList());
	}

	// 11. Sum of Digits of a Number
	public int sumOfDigits(int num) {
		int sum = 0;
		while (num != 0) {
			sum += num % 10;
			num /= 10;
		}
		return sum;
	}

	// 12. Reverse of a Number
	public int reverseNumber(int num) {
		int reversed = 0;
		while (num != 0) {
			reversed = reversed * 10 + num % 10;
			num /= 10;
		}
		return reversed;
	}

	// 13. Palindrome Number
	public boolean isPalindrome(int num) {
		return num == reverseNumber(num);
	}

	// 14. Armstrong Number
	public boolean isArmstrong(int num) {
		int sum = 0, original = num;
		int digits = String.valueOf(num).length();
		while (num != 0) {
			sum += Math.pow(num % 10, digits);
			num /= 10;
		}
		return sum == original;
	}

	// 15. Armstrong Numbers in a Given Range
	public List<Integer> armstrongNumbersInRange(int start, int end) {
		return IntStream.rangeClosed(start, end).filter(this::isArmstrong).boxed().collect(Collectors.toList());
	}

	// 16. Fibonacci Series up to Nth Term // Steam API 
	public List<Integer> fibonacciSeries(int n) {
		List<Integer> series = new ArrayList<>(Arrays.asList(0, 1));
		IntStream.range(2, n).forEach(i -> series.add(series.get(i - 1) + series.get(i - 2)));
		return series.subList(0, n);
	}

	// 17. Find the Nth Term of the Fibonacci Series // Recursion
	static int nthFibonacciSeries(int n) {
		if (n <= 1)
			return n;
		return nthFibonacciSeries(n - 1) + nthFibonacciSeries(n - 2);
	}

	public List<Integer> nthFibonacci(int n) {
		List<Integer> series = new ArrayList<>();
		for (int i = 0; i < n; i++) {
			series.add(nthFibonacciSeries(i));
		}
		return series;
	}

	// 18. Factorial of a Number
	public long factorial(int num) {
		return IntStream.rangeClosed(1, num).reduce(1, (a, b) -> a * b);
	}

	// 19. Power of a Number
	public double power(int base, int exp) {
		return Math.pow(base, exp);
	}

	// 20. Factor of a Number
	public List<Integer> factors(int num) {
		return IntStream.rangeClosed(1, num).filter(i -> num % i == 0).boxed().collect(Collectors.toList());
	}

	// 21. Prime Factors of a Number 
	public List<Integer> primeFactors(int num) {
		List<Integer> factors = new ArrayList<>();
		for (int i = 2; i <= num; i++) {
			while (num % i == 0) {
				factors.add(i);
				num /= i;
			}
		}
		return factors;
	}

	// 22. Strong Number
	public boolean isStrongNumber(int num) {
		int sum = 0, original = num;
		while (num != 0) {
			int digit = num % 10;
			sum += factorial(digit);
			num /= 10;
		}
		return sum == original;
	}

	// 23. Perfect Number
	public boolean isPerfectNumber(int num) {
		return IntStream.rangeClosed(1, num / 2).filter(i -> num % i == 0).sum() == num;
	}

	// 24. Perfect Square
	public boolean isPerfectSquare(int num) {
		int sqrt = (int) Math.sqrt(num);
		return sqrt * sqrt == num;
	}

	// 25. Automorphic Number
	public boolean isAutomorphic(int num) {
		int square = num * num;
		return String.valueOf(square).endsWith(String.valueOf(num));
	}

	// 26. Harshad Number
	public boolean isHarshadNumber(int num) {
		return num % sumOfDigits(num) == 0;
	}

	// 27. Abundant Number // 12, 18, 20, 24
	public boolean isAbundantNumber(int num) {
		int sumOfDivisors = IntStream.rangeClosed(1, num / 2).filter(i -> num % i == 0).sum();
		return sumOfDivisors > num;
	}

	// 28. Friendly Pair
	public boolean isFriendlyPair(int num1, int num2) {
		int sum1 = IntStream.rangeClosed(1, num1 / 2).filter(i -> num1 % i == 0).sum();
		int sum2 = IntStream.rangeClosed(1, num2 / 2).filter(i -> num2 % i == 0).sum();
		return sum1 == num2 && sum2 == num1;
	}

	// Number Programs

	// 1. Highest Common Factor (HCF) /
	public int hcf(int a, int b) {
		return b == 0 ? a : hcf(b, a % b);
	}

	// 2. Lowest Common Multiple (LCM)
	public int lcm(int a, int b) {
		return (a * b) / gcd(a, b);
	}

	// 3. Greatest Common Divisor (GCD)
	public int gcd(int a, int b) {
		// Find Minimum of a and b
		int result = Math.min(a, b);
		while (result > 0) {
			if (a % result == 0 && b % result == 0) {
				break;
			}
			result--;
		}

		// Return gcd of a and b
		return result;
	}

	// 4. Binary to Decimal Conversion
	public int binaryToDecimal(String binary) {
		return Integer.parseInt(binary, 2);
	}

	// 5. Octal to Decimal Conversion
	public int octalToDecimal(String octal) {
		return Integer.parseInt(octal, 8);
	}

	// 6. Hexadecimal to Decimal Conversion
	public int hexToDecimal(String hex) {
		return Integer.parseInt(hex, 16);
	}

	// 7. Decimal to Binary Conversion
	public String decimalToBinary(int decimal) {
		return Integer.toBinaryString(decimal);
	}

	// 8. Decimal to Octal Conversion
	public String decimalToOctal(int decimal) {
		return Integer.toOctalString(decimal);
	}

	// 9. Decimal to Hexadecimal Conversion
	public String decimalToHex(int decimal) {
		return Integer.toHexString(decimal).toUpperCase();
	}

	// 10. Binary to Octal Conversion
	public String binaryToOctal(String binary) {
		int decimal = binaryToDecimal(binary);
		return decimalToOctal(decimal);
	}

	// 11. Octal to Binary Conversion
	public String octalToBinary(String octal) {
		int decimal = octalToDecimal(octal);
		return decimalToBinary(decimal);
	}

	// 12. Quadrants in which a given coordinate lies
	public String findQuadrant(int x, int y) {
		if (x > 0 && y > 0)
			return "1st Quadrant";
		else if (x < 0 && y > 0)
			return "2nd Quadrant";
		else if (x < 0 && y < 0)
			return "3rd Quadrant";
		else if (x > 0 && y < 0)
			return "4th Quadrant";
		else
			return "On Axis";
	}

	// 13. Permutations of n people occupying r seats
	public long permutations(int n, int r) {
		return factorial(n) / factorial(n - r);
	}

	// 14. Maximum number of handshakes
	public int maxHandshakes(int n) {
		return n * (n - 1) / 2;
	}

	// 15. Addition of two fractions
	public String addFractions(int num1, int den1, int num2, int den2) {
		int lcm = lcm(den1, den2);
		int sum = num1 * (lcm / den1) + num2 * (lcm / den2);
		int hcf = hcf(sum, lcm);
		return (sum / hcf) + "/" + (lcm / hcf);
	}

	// 16. Replace all 0’s with 1 in a given integer
	public int replaceZeroWithOne(int num) {
		return Integer.parseInt(String.valueOf(num).replace('0', '1'));
	}

	// 17. Can a number be expressed as a sum of two prime numbers
	public boolean canBeSumOfTwoPrimes(int num) {
		for (int i = 2; i <= num / 2; i++) {
			if (isPrime(i) && isPrime(num - i))
				return true;
		}
		return false;
	}

	// 18. Count possible decodings of a given digit sequence (A=1, B=2, ..., Z=26)
	public int countDecodings(String digits) {
		if (digits.isEmpty() || digits.startsWith("0"))
			return 0;
		int[] dp = new int[digits.length() + 1];
		dp[0] = 1;
		dp[1] = digits.charAt(0) != '0' ? 1 : 0;
		for (int i = 2; i <= digits.length(); i++) {
			int oneDigit = Integer.parseInt(digits.substring(i - 1, i));
			int twoDigits = Integer.parseInt(digits.substring(i - 2, i));
			if (oneDigit >= 1)
				dp[i] += dp[i - 1];
			if (twoDigits >= 10 && twoDigits <= 26)
				dp[i] += dp[i - 2];
		}
		return dp[digits.length()];
	}

	// 19. Calculate the area of a circle
	public double areaOfCircle(double radius) {
		return Math.PI * radius * radius;
	}

	// 20. Find prime numbers between 1 to 100
	public List<Integer> primesUpTo100() {
		return IntStream.rangeClosed(2, 100).filter(this::isPrime).boxed().collect(Collectors.toList());
	}

	// 21. Calculate the number of digits in an integer
	public int countDigits(int num) {
		return String.valueOf(Math.abs(num)).length();
	}

	// 22. Convert digit/number to words (up to 9999)
	public String numberToWords(int num) {
		String[] units = { "", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine" };
		String[] tens = { "", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety" };
		String[] teens = { "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen",
				"Eighteen", "Nineteen" };

		if (num < 10)
			return units[num];
		else if (num < 20)
			return teens[num - 10];
		else if (num < 100)
			return tens[num / 10] + " " + units[num % 10];
		else if (num < 1000)
			return units[num / 100] + " Hundred " + (num % 100 != 0 ? numberToWords(num % 100) : "");
		else
			return units[num / 1000] + " Thousand " + (num % 1000 != 0 ? numberToWords(num % 1000) : "");
	}

	// 23. Counting number of days in a given month of a year
	public int daysInMonth(int month, int year) {
		YearMonth yearMonth = YearMonth.of(year, month);
		return yearMonth.lengthOfMonth();
	}

	// 24. Finding number of times x digit occurs in a given input // INFO: chars()
	public long countDigitOccurrences(int num, int digit) {
		return String.valueOf(num).chars().filter(c -> c == ('0' + digit)).count();
	}

	// 25. Finding number of integers with exactly x divisors up to a limit
	public long countNumbersWithExactDivisors(int limit, int x) {
		return IntStream.rangeClosed(1, limit)
				.filter(num -> IntStream.rangeClosed(1, num).filter(i -> num % i == 0).count() == x).count();
	}

	// 26. Finding roots of a quadratic equation ax^2 + bx + c = 0
	public String findQuadraticRoots(double a, double b, double c) {
		double discriminant = b * b - 4 * a * c;
		if (discriminant > 0) {
			double root1 = (-b + Math.sqrt(discriminant)) / (2 * a);
			double root2 = (-b - Math.sqrt(discriminant)) / (2 * a);
			return "Two real roots: " + root1 + " and " + root2;
		} else if (discriminant == 0) {
			double root = -b / (2 * a);
			return "One real root: " + root;
		} else {
			return "Complex roots";
		}
	}

	// Recursion Programs
	// 3. Largest Element in an Array // INFO: Arrays.stream(arr)
	public int largestElement(int[] arr) {
		return Arrays.stream(arr).max().orElseThrow();
	}

	// 4. Smallest Element in an Array
	public int smallestElement(int[] arr) {
		return Arrays.stream(arr).min().orElseThrow();
	}

	// 8. Calculate Length of String Using Recursion
	public int lengthOfString(String str) {
		if (str.isEmpty())
			return 0;
		return 1 + lengthOfString(str.substring(1));
	}

	// 9. Print All Permutations of a String
	public void permutations(String str, String result) {
		if (str.isEmpty())
			System.out.println(result);
		for (int i = 0; i < str.length(); i++) {
			permutations(str.substring(0, i) + str.substring(i + 1), result + str.charAt(i));
		}
	}

	// 10. Print F(N)th term (Assuming F(N) is Fibonacci)
	public int fibonacci(int n) {
		if (n <= 1)
			return n;
		return fibonacci(n - 1) + fibonacci(n - 2);
	}

	// 11. Sums of All Subsets in a List
	public List<Integer> subsetSums(List<Integer> arr) {
		List<Integer> result = new ArrayList<>();
		subsetSumsHelper(arr, 0, 0, result);
		return result;
	}

	private void subsetSumsHelper(List<Integer> arr, int index, int sum, List<Integer> result) {
		if (index == arr.size()) {
			result.add(sum);
			return;
		}
		subsetSumsHelper(arr, index + 1, sum + arr.get(index), result);
		subsetSumsHelper(arr, index + 1, sum, result);
	}

	// 12. Last Non-Zero Digit in Factorial
	public int lastNonZeroDigitInFactorial(int n) {
		int result = 1;
		for (int i = 1; i <= n; i++) {
			result *= i;
			while (result % 10 == 0)
				result /= 10;
			result %= 1000; // Keep result small to avoid overflow
		}
		return result % 10;
	}

	// 13. Nth Row of Pascal’s Triangle
	public List<Integer> nthRowOfPascalsTriangle(int n) {
		List<Integer> row = new ArrayList<>(Collections.singletonList(1));
		for (int k = 1; k <= n; k++) {
			row.add((int) ((long) row.get(k - 1) * (n - k + 1) / k));
		}
		return row;
	}

	// 14. Generate All Combinations of Well-formed Parentheses
	public void generateParentheses(int n) {
		generateParenthesesHelper("", n, n);
	}

	private void generateParenthesesHelper(String s, int left, int right) {
		if (left == 0 && right == 0) {
			System.out.println(s);
			return;
		}
		if (left > 0)
			generateParenthesesHelper(s + "(", left - 1, right);
		if (right > left)
			generateParenthesesHelper(s + ")", left, right - 1);
	}

	// 16. Find All Palindromic Partitions of a String // doubt
	public void palindromicPartitions(String str) {
		palindromicPartitionsHelper(str, new ArrayList<>());
	}

	private void palindromicPartitionsHelper(String str, List<String> result) {
		if (str.isEmpty()) {
			System.out.println(result);
			return;
		}
		for (int i = 1; i <= str.length(); i++) {
			String prefix = str.substring(0, i);
			if (isPalindrome(prefix)) {
				result.add(prefix);
				palindromicPartitionsHelper(str.substring(i), result);
				result.remove(result.size() - 1);
			}
		}
	}

	private boolean isPalindrome(String s) {
		return s.equals(new StringBuilder(s).reverse().toString());
	}

	// 17. Find All N-Bit Binary Numbers with More or Equal 1’s than 0’s
	public void nBitBinaryNumbers(int n) {
		generateBinary("", 0, 0, n);
	}

	private void generateBinary(String str, int ones, int zeros, int n) {
		if (str.length() == n) {
			System.out.println(str);
			return;
		}
		generateBinary(str + "1", ones + 1, zeros, n);
		if (ones > zeros)
			generateBinary(str + "0", ones, zeros + 1, n);
	}

	// 18. Find All Subsets of a Set
	public void allSubsets(List<Integer> set) {
		generateSubsets(set, 0, new ArrayList<>());
	}

	private void generateSubsets(List<Integer> set, int index, List<Integer> subset) {
		if (index == set.size()) {
			System.out.println(subset);
			return;
		}
		subset.add(set.get(index));
		generateSubsets(set, index + 1, subset);
		subset.remove(subset.size() - 1);
		generateSubsets(set, index + 1, subset);
	}

	// 19. Remove All Adjacent Duplicate Characters Recursively
	public String removeAdjacentDuplicates(String s) {
		StringBuilder sb = new StringBuilder();
		for (int i = 0; i < s.length(); i++) {
			if (sb.length() > 0 && sb.charAt(sb.length() - 1) == s.charAt(i)) {
				sb.deleteCharAt(sb.length() - 1);
			} else {
				sb.append(s.charAt(i));
			}
		}
		String result = sb.toString();
		return result.equals(s) ? result : removeAdjacentDuplicates(result);
	}

	// Arrays Programs

	// 1. Find Largest Element in an Array
	public int findLargest(int[] arr) {
		return Arrays.stream(arr).max().orElseThrow();
	}

	// 2. Find Smallest Element in an Array
	public int findSmallest(int[] arr) {
		return Arrays.stream(arr).min().orElseThrow();
	}

	// 3. Find the Smallest and Largest Element in an Array
	public int[] findSmallestAndLargest(int[] arr) {
		int min = Arrays.stream(arr).min().orElseThrow();
		int max = Arrays.stream(arr).max().orElseThrow();
		return new int[] { min, max };
	}

	// 4. Find Second Smallest Element in an Array
	public int findSecondSmallest(int[] arr) {
		return Arrays.stream(arr).distinct().sorted().skip(1).findFirst().orElseThrow();
	}

	// 5. Calculate the Sum of Elements in an Array
	public int sumArray(int[] arr) {
		return Arrays.stream(arr).sum();
	}

	// 6. Reverse an Array
	public int[] reverseArray(int[] arr) {
		for (int i = 0, j = arr.length - 1; i < j; i++, j--) {
			int temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
		return arr;
	}

	// 7. Sort First Half in Ascending and Second Half in Descending
	public int[] sortHalf(int[] arr) {
		int mid = arr.length / 2;
		Arrays.sort(arr, 0, mid);
		Arrays.sort(arr, mid, arr.length);
		for (int i = mid, j = arr.length - 1; i < j; i++, j--) {
			int temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
		return arr;
	}

	// 8. Sort the Elements of an Array
	public int[] sortArray(int[] arr) {
		return Arrays.stream(arr).sorted().toArray();
	}

	// 9. Finding the Frequency of Elements in an Array
	public Map<Integer, Long> frequencyOfElements(int[] arr) {
		return Arrays.stream(arr).boxed().collect(Collectors.groupingBy(e -> e, Collectors.counting()));
	}

	// 10. Sorting Elements of an Array by Frequency // Hard
	public List<Integer> sortByFrequency(int[] arr) {
		return Arrays.stream(arr).boxed().collect(Collectors.groupingBy(e -> e, Collectors.counting())).entrySet()
				.stream()
				.sorted((a, b) -> !a.getValue().equals(b.getValue()) ? b.getValue().compareTo(a.getValue())
						: a.getKey().compareTo(b.getKey()))
				.flatMap(e -> Collections.nCopies(Math.toIntExact(e.getValue()), e.getKey()).stream())
				.collect(Collectors.toList());
	}

	// 11. Finding the Longest Palindrome in an Array
	public String longestPalindrome(String[] arr) {
		return Arrays.stream(arr).filter(s -> s.equals(new StringBuilder(s).reverse().toString()))
				.max(Comparator.comparingInt(String::length)).orElse("");
	}

	// 12. Counting Distinct Elements in an Array
	public long countDistinct(int[] arr) {
		return Arrays.stream(arr).distinct().count();
	}

	// 13. Finding Repeating Elements in an Array
	public Set<Integer> findRepeating(int[] arr) {
		Set<Integer> allItems = new HashSet<>();
		return Arrays.stream(arr).filter(n -> !allItems.add(n)).boxed().collect(Collectors.toSet());
	}

	// 14. Finding Non-Repeating Elements in an Array
	public List<Integer> findNonRepeating(int[] arr) {
		Map<Integer, Long> freqMap = Arrays.stream(arr).boxed()
				.collect(Collectors.groupingBy(e -> e, Collectors.counting()));
		return freqMap.entrySet().stream().filter(e -> e.getValue() == 1).map(Map.Entry::getKey)
				.collect(Collectors.toList());
	}

	// 15. Removing Duplicate Elements from an Array
	public int[] removeDuplicates(int[] arr) {
		return Arrays.stream(arr).distinct().toArray();
	}

	// 16. Finding Minimum Scalar Product of Two Vectors
	public int minScalarProduct(int[] a, int[] b) {
		Arrays.sort(a);
		Arrays.sort(b);
		int sum = 0;
		for (int i = 0; i < a.length; i++) {
			sum += a[i] * b[b.length - 1 - i];
		}
		return sum;
	}

	// 17. Finding Maximum Scalar Product of Two Vectors
	public int maxScalarProduct(int[] a, int[] b) {
		Arrays.sort(a);
		Arrays.sort(b);
		int sum = 0;
		for (int i = 0; i < a.length; i++) {
			sum += a[i] * b[i];
		}
		return sum;
	}

	// 18. Counting the Number of Even and Odd Elements in an Array
	public int[] countEvenOdd(int[] arr) {
		int evens = (int) Arrays.stream(arr).filter(n -> n % 2 == 0).count();
		int odds = arr.length - evens;
		return new int[] { evens, odds };
	}

	// 19. Find All Symmetric Pairs in an Array
	public List<int[]> findSymmetricPairs(int[][] arr) {
		Set<String> set = new HashSet<>();
		List<int[]> result = new ArrayList<>();
		for (int[] pair : arr) {
			String reverse = pair[1] + "," + pair[0];
			if (set.contains(reverse)) {
				result.add(new int[] { pair[1], pair[0] });
			}
			set.add(pair[0] + "," + pair[1]);
		}
		return result;
	}

	// 20. Find Maximum Product Sub-array in a Given Array // Doubt
	public int maxProductSubArray(int[] arr) {
		int max = arr[0], min = arr[0], result = arr[0];
		for (int i = 1; i < arr.length; i++) {
			if (arr[i] < 0) {
				int temp = max;
				max = min;
				min = temp;
			}
			max = Math.max(arr[i], max * arr[i]);
			min = Math.min(arr[i], min * arr[i]);
			result = Math.max(result, max);
		}
		return result;
	}

	// 21. Finding Arrays are Disjoint or Not
	public boolean areDisjoint(int[] arr1, int[] arr2) {
		Set<Integer> set = Arrays.stream(arr1).boxed().collect(Collectors.toSet());
		return Arrays.stream(arr2).noneMatch(set::contains);
	}

	// 22. Determine if an Array is a Subset of Another Array
	public boolean isSubset(int[] arr1, int[] arr2) {
		Set<Integer> set = Arrays.stream(arr1).boxed().collect(Collectors.toSet());
		return Arrays.stream(arr2).allMatch(set::contains);
	}

	// 23. Determine if All Numbers in an Array Can Be Made Equal
	public boolean canMakeEqual(int[] arr) {
		int gcd = arr[0];
		for (int i = 1; i < arr.length; i++) {
			gcd = gcd(gcd, arr[i]);
		}
		return gcd == 1;
	}

	// 24. Finding Minimum Sum of Absolute Difference of Array // doubt
	public int minSumOfAbsoluteDifference(int[] arr) {
		Arrays.sort(arr);
		int median = arr[arr.length / 2];
		return Arrays.stream(arr).map(n -> Math.abs(n - median)).sum();
	}

	// 26. Replace Each Element of Array by Its Rank
	public int[] replaceWithRank(int[] arr) {
		int[] sorted = Arrays.stream(arr).sorted().distinct().toArray();
		Map<Integer, Integer> ranks = IntStream.range(0, sorted.length).boxed()
				.collect(Collectors.toMap(i -> sorted[i], i -> i + 1));
		return Arrays.stream(arr).map(ranks::get).toArray();
	}

	// Operations on Strings

	// 1. Check whether a character is a vowel or consonant
	public String checkVowelOrConsonant(char ch) {
		return "AEIOUaeiou".indexOf(ch) >= 0 ? "Vowel" : "Consonant";
	}

	// 2. Check whether a character is an alphabet or not
	public boolean isAlphabet(char ch) {
		return Character.isLetter(ch);
	}

	// 3. Find the ASCII value of a character
	public int asciiValue(char ch) {
		return (int) ch;
	}

	// 4. Length of the string without using length() function
	public int stringLength(String str) {
		return str.chars().map(c -> 1).sum();
	}

	// 5. Toggle each character in a string
	public String toggleCase(String str) {
		return str.chars().mapToObj(c -> Character.isUpperCase(c) ? Character.toLowerCase(c) : Character.toUpperCase(c))
				.map(String::valueOf).collect(Collectors.joining());
	}

	// 6. Count the number of vowels
	public long countVowels(String str) {
		return str.chars().filter(c -> "AEIOUaeiou".indexOf(c) >= 0).count();
	}

	// 7. Remove the vowels from a String
	public String removeVowels(String str) {
		return str.replaceAll("[AEIOUaeiou]", "");
	}

	// 9. Print the given string in reverse order
	public String reverseString(String str) {
		return new StringBuilder(str).reverse().toString();
	}

	// 10. Remove all characters from a string except alphabets
	public String removeNonAlphabets(String str) {
		return str.replaceAll("[^A-Za-z]", "");
	}

	// 11. Remove spaces from a string
	public String removeSpaces(String str) {
		return str.replaceAll("\\s+", "");
	}

	// 12. Remove brackets from an algebraic expression
	public String removeBrackets(String str) {
		return str.replaceAll("[(){}\\[\\]]", "");
	}

	// 13. Count the sum of numbers in a string
	public int sumOfNumbersInString(String str) {
		return Arrays.stream(str.split("\\D+")).filter(s -> !s.isEmpty()).mapToInt(Integer::parseInt).sum();
	}

	// 14. Capitalize the first and last character of each word in a string
	public String capitalizeFirstAndLast(String str) {
		return Arrays.stream(str.split("\\s+"))
				.map(word -> word.length() > 1 ? Character.toUpperCase(word.charAt(0))
						+ word.substring(1, word.length() - 1) + Character.toUpperCase(word.charAt(word.length() - 1))
						: word.toUpperCase())
				.collect(Collectors.joining(" "));
	}

	// 15. Calculate frequency of characters in a string
	public Map<Character, Long> characterFrequency(String str) {
		return str.chars().mapToObj(c -> (char) c).collect(Collectors.groupingBy(c -> c, Collectors.counting()));
	}

	// 17. Check if two strings are Anagram or not
	public boolean isAnagram(String s1, String s2) {
		return s1.length() == s2.length() && s1.chars().sorted().boxed().collect(Collectors.toList())
				.equals(s2.chars().sorted().boxed().collect(Collectors.toList()));
	}

	// 18. Replace a sub-string in a string
	public String replaceSubstring(String str, String oldSub, String newSub) {
		return str.replace(oldSub, newSub);
	}

	// 19. Replace a particular word with another word in a string
	public String replaceWord(String str, String oldWord, String newWord) {
		return Arrays.stream(str.split("\\s+")).map(word -> word.equals(oldWord) ? newWord : word)
				.collect(Collectors.joining(" "));
	}

	// 20. Count common sub-sequences in two strings // doubt
	public int countCommonSubsequences(String s1, String s2) {
		Set<String> set1 = getSubsequences(s1);
		Set<String> set2 = getSubsequences(s2);
		set1.retainAll(set2);
		return set1.size();
	}

	private Set<String> getSubsequences(String str) {
		Set<String> subsequences = new HashSet<>();
		int n = str.length();
		for (int i = 0; i < (1 << n); i++) {
			StringBuilder sb = new StringBuilder();
			for (int j = 0; j < n; j++) {
				if ((i & (1 << j)) > 0) {
					sb.append(str.charAt(j));
				}
			}
			if (sb.length() > 0) {
				subsequences.add(sb.toString());
			}
		}
		return subsequences;
	}

	// 21. Check if two strings match where one string contains wildcard characters
	public boolean wildcardMatch(String str, String pattern) {
		return str.matches(pattern.replace("?", ".?").replace("*", ".*"));
	}

	// 22. Print all permutations of a given string in lexicographically sorted
	// order // doubt
	public List<String> countCommonSubsequences(String str) {
		List<String> permutations = new ArrayList<>();
		generatePermutations(str.toCharArray(), 0, permutations);
		Collections.sort(permutations);
		return permutations;
	}

	private void generatePermutations(char[] arr, int currentIndex, List<String> permutations) {
		if (currentIndex == arr.length - 1) {
			permutations.add(new String(arr));
		} else {
			for (int i = currentIndex; i < arr.length; i++) {
				swap(arr, currentIndex, i);
				generatePermutations(arr, currentIndex + 1, permutations);
				swap(arr, currentIndex, i); // backtrack
			}
		}
	}

	private void swap(char[] arr, int i, int j) {
		char temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}

	// Stream Examples
	
	public static List<Integer> findEvenNumbers(List<Integer> numbers) {
		return numbers.stream().filter(n -> n % 2 == 0).collect(Collectors.toList());
	}
	
	public static List<Integer> arrayToList(int[] array) {
	    return Arrays.stream(array).boxed().collect(Collectors.toList());
	}

	public static int sumOfNumbers(List<Integer> numbers) {
		return numbers.stream().mapToInt(Integer::intValue).sum();
	}

	public static Optional<Integer> findMin(List<Integer> numbers) {
		return numbers.stream().min(Integer::compareTo);
	}

	public static List<String> convertToUpperCase(List<String> strings) {
		return strings.stream().map(String::toUpperCase).collect(Collectors.toList());
	}

	public static List<Integer> sortAscending(List<Integer> numbers) {
		return numbers.stream().sorted().collect(Collectors.toList());
	}

	public static List<Integer> sortDescending(List<Integer> numbers) {
		return numbers.stream().sorted(Comparator.reverseOrder()).collect(Collectors.toList());
	}

	public static List<Integer> findDuplicates(List<Integer> numbers) {
		return numbers.stream().filter(n -> Collections.frequency(numbers, n) > 1).distinct()
				.collect(Collectors.toList());
	}

	public static long countElements(List<Integer> numbers) {
		return numbers.stream().count();
	}

	public static List<String> filterStrings(List<String> strings) {
		return strings.stream().filter(s -> s.startsWith("A")).collect(Collectors.toList());
	}

	public static List<Integer> flattenListOfLists(List<List<Integer>> listOfLists) {
		return listOfLists.stream().flatMap(List::stream).collect(Collectors.toList());
	}

	public static Optional<Integer> findFirst(List<Integer> numbers) {
		return numbers.stream().findFirst();
	}

	public static List<Integer> findDistinctElements(List<Integer> numbers) {
		return numbers.stream().distinct().collect(Collectors.toList());
	}

	public static String joinStrings(List<String> strings, String delimiter) {
		return strings.stream().collect(Collectors.joining(delimiter));
	}

	public static List<Integer> processInParallel(List<Integer> numbers) {
		return numbers.parallelStream().map(n -> n * 2).collect(Collectors.toList());
	}

	public static Optional<Integer> findSecondLargest(List<Integer> numbers) {
		return numbers.stream().sorted(Comparator.reverseOrder()).skip(1).findFirst();
	}

	public static Integer[] convertToArray(List<Integer> numbers) {
		return numbers.stream().toArray(Integer[]::new);
	}

	public static List<String> filterNullValues(List<String> strings) {
		return strings.stream().filter(Objects::nonNull).collect(Collectors.toList());
	}

	public static List<Integer> limitElements(List<Integer> numbers, int limit) {
		return numbers.stream().limit(limit).collect(Collectors.toList());
	}

	public static int productOfElements(List<Integer> numbers) {
		return numbers.stream().reduce(1, (a, b) -> a * b);
	}

	public static List<Integer> filterNumbers(List<Integer> numbers) {
		Predicate<Integer> isGreaterThan10 = n -> n > 10;
		return numbers.stream().filter(isGreaterThan10).collect(Collectors.toList());
	}

	public static List<String> convertToString(List<Integer> numbers) {
		Function<Integer, String> intToString = String::valueOf;
		return numbers.stream().map(intToString).collect(Collectors.toList());
	}

	public static void printElements(List<Integer> numbers) {
		Consumer<Integer> print = System.out::println;
		numbers.forEach(print);
	}

	public static int sumOfOddNumbers(List<Integer> numbers) {
		Predicate<Integer> isOdd = n -> n % 2 != 0;
		return numbers.stream().filter(isOdd).mapToInt(Integer::intValue).sum();
	}

	public static int addTwoNumbers(int a, int b) {
		BiFunction<Integer, Integer, Integer> add = Integer::sum;
		return add.apply(a, b);
	}

	public static int generateRandomNumber() {
		Supplier<Integer> randomSupplier = () -> new Random().nextInt(100);
		return randomSupplier.get();
	}

	public static List<String> sortStrings(List<String> strings) {
		return strings.stream().sorted(Comparator.reverseOrder()).collect(Collectors.toList());
	}

	public static boolean allEven(List<Integer> numbers) {
		Predicate<Integer> isEven = n -> n % 2 == 0;
		return numbers.stream().allMatch(isEven);
	}

	public static List<String> filterAndTransform(List<String> strings) {
		return strings.stream().filter(s -> s.length() >= 3).map(String::toUpperCase).collect(Collectors.toList());
	}

	public static String getValue(Map<String, String> map, String key) {
		return Optional.ofNullable(map.get(key)).orElse("Default Value");
	}

	public static Map<Boolean, List<Integer>> groupByOddEven(List<Integer> numbers) {
		return numbers.stream().collect(Collectors.groupingBy(n -> n % 2 == 0));
	}
	
	public static Map<Boolean, List<Integer>> groupByEvenOdd(List<Integer> numbers) {
		return numbers.stream().collect(Collectors.partitioningBy(n -> n % 2 == 0));
	}

	public static Optional<String> findLongestWord(List<String> words) {
		return words.stream().max(Comparator.comparingInt(String::length));
	}

	public static double calculateAverage(List<Integer> numbers) {
		return numbers.stream().mapToInt(Integer::intValue).average().orElse(0.0);
	}
	
	public static Map<String, Integer> listToMap(List<Employee> employees) {
		return employees.stream().collect(Collectors.toMap(Employee::getName, Employee::getSalary));
	}

	public static List<String> listEmpName(List<Employee> employees) {
		return employees.stream().map(Employee::getName).collect(Collectors.toList());
	}
	
	
	public static List<Integer> findIntersection(List<Integer> list1, List<Integer> list2) {
	    list1.retainAll(list2);
	    return list1;
	}

	public static List<Integer> findUnion(List<Integer> list1, List<Integer> list2) {
	    Set<Integer> set = new HashSet<>(list1);
	    set.addAll(list2);
	    return new ArrayList<>(set);
	}

	public static List<Integer> removeDuplicates(List<Integer> list) {
	    return new ArrayList<>(new LinkedHashSet<>(list));
	}

	public static void sortEmployeesByAge(List<Employee> employees) {
	    employees.sort(Comparator.comparingInt(Employee::getAge));
	}

	public static boolean areMapsEqual(Map<String, Integer> map1, Map<String, Integer> map2) {
	    return map1.equals(map2);
	}

	public static Map<Integer, String> convertListToMap(List<Employee> employees) {
	    return employees.stream().collect(Collectors.toMap(Employee::getId, Employee::getName));
	}

	public static Map<String, Integer> sortByValues(Map<String, Integer> map) {
	    return map.entrySet()
	              .stream()
	              .sorted(Map.Entry.comparingByValue())
	              .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, (e1, e2) -> e1, LinkedHashMap::new));
	}

	public static boolean hasDuplicates(List<Integer> list) {
	    Set<Integer> set = new HashSet<>(list);
	    return set.size() != list.size();
	}

	public static boolean isSubset(List<Integer> list1, List<Integer> list2) {
	    return list2.containsAll(list1);
	}

	public static Set<Integer> listToSet(List<Integer> list) {
	    return new HashSet<>(list);
	}

	public static List<Integer> mergeLists(List<Integer> list1, List<Integer> list2) {
	    list1.addAll(list2);
	    return list1;
	}

	public static int findMax(List<Integer> list) {
	    return Collections.max(list);
	}

	public static List<Integer> mapKeysToList(Map<Integer, String> map) {
	    return new ArrayList<>(map.keySet());
	}

	public static List<String> mapValuesToList(Map<Integer, String> map) {
	    return new ArrayList<>(map.values());
	}

	public static boolean searchElement(List<Integer> numbers, int target) {
	    return numbers.contains(target);
	}

	public static int binarySearch(List<Integer> sortedNumbers, int target) {
	    return Collections.binarySearch(sortedNumbers, target);
	}

	public static boolean compareLists(List<Integer> list1, List<Integer> list2) {
	    return list1.equals(list2);
	}

	public static List<Integer> removeIfLessThan(List<Integer> numbers, int threshold) {
	    return numbers.stream()
	                  .filter(n -> n >= threshold)
	                  .collect(Collectors.toList());
	}
	
//	public static void main(String args[]) {
//		List<Employee> employees = new ArrayList<Employee>();
//		employees.add(new Employee("Alice", 1001, 15, "CS", 30000));
//		employees.add(new Employee("Bob", 2001, 25, "EC", 40000));
//		employees.add(new Employee("Charlie", 3001, 35, "EE", 50000));
//		employees.add(new Employee("David", 4001, 45, "CS", 60000));
//		employees.add(new Employee("Ram", 5001, 33, "CS", 44000));
//		employees.add(new Employee("Krishna", 6001, 38, "ME", 25000));
//		
//		
//		System.out.println(listEmpName(employees).toString());
//		System.out.println(listToMap(employees).entrySet());
//		
//		TopStream ts = new TopStream();
//		int num = 2;
//		String s = "AB";
//
//		String[] arrStr = { "AB", "ADA", "APPA" };
//
//		int[] arr = { 5, 2, 1, -8, -2, -6, 9 };
//
//		ts.nBitBinaryNumbers(4);
//
//		List<Integer> integerList = Arrays.stream(arr).boxed().collect(Collectors.toList());
//
//		System.out.println(s.chars().sorted().boxed().collect(Collectors.toList()).equals(Arrays.asList(65, 66)));
//		System.out.println(s.chars().sorted().boxed().collect(Collectors.toList()));
//		
//		
//		System.out.println(ts.countCommonSubsequences(s));
//	}
	
	public static void main(String args[]) {
		TopStream ts = new TopStream();
		
	    System.out.print(ts.primeFactors(12));
	}

}

```
