import java.util.Arrays;
import java.util.Random;

/**
 * merge sort sample 
 * 
 * 
 * 
 */


public class MergeSort {

	/**
	 * recursive part of MergeSort. This method returns the elements between
	 * start and end in sorted order.
	 * 
	 * @param S
	 *            array of elements to sort
	 * @param start
	 *            first element of S to be included in sort (inclusive)
	 * @param end
	 *            last element of S to be included in sort (*exclusive*)
	 * @return array of length start-end that contains the elements of S between
	 *         start and end in sorted order.
	 */
	public static int[] MSort(int[] S, final int start, final int end) {

		// if nothing to sort:
		if (end - start == 1) {
			int[] E = new int[1];
			E[0] = S[start];
			return E;
		}

		// split list in two O(1), note: integer devision
		final int split = start + (end - start) / 2;

		// recursion 2*T(n/2)
		int[] R = MSort(S, start, split);
		int[] T = MSort(S, split, end);
		
		
		S = new int[end - start];

		// combine:
		int t = 0;
		int r = 0;

		/*
		 * broken: we do not handle if t or s reach the end. for(int s = 0; s <
		 * sorted.length; s++) {
		 * 
		 * S[s] = (T[t] < R[r]) ? T[t++] : R[r++]; }
		 */

		// this works:
		while (t < T.length && r < R.length) {
			S[t + r] = (T[t] < R[r]) ? T[t++] : R[r++];
		}
		// deal with remaining elements if any;
		while (t < T.length) {
			S[t + r] = T[t++];
		}
		while (r < R.length) {
			S[t + r] = R[r++];
		}

		return S;
	}

	/**
	 * For testing purposes: - generate some random data to sort and check
	 * against Java's own library function whether our implementation yields the
	 * same results.
	 * 
	 * @param args
	 *            command line arguments -- unused
	 */
	public static void main(String[] args) {

		// allocate two arrays
		int[] s = new int[1000];
		int[] t = new int[s.length];

		// and a random number generator;
		Random r = new Random();

		// count the number of errors our algorithms makes
		int error_count = 0;

		// do a 1000 times?:
		for (int i = 0; i < 1000; i++) {
			// create new random sequence of 1000 numbers and fill both arrays
			// identically:
			for (int j = 0; j < s.length; j++) {
				t[j] = s[j] = r.nextInt(1000000);
			}

			// for(int k: s) {
			// System.out.print(k + " ");
			// }
			// System.out.println();

			// sort s using our implementation:
			s = MSort(s, 0, s.length);

			// sort t using Java's implementation:
			Arrays.sort(t);

			// check whether results are the same for both algorithms
			if (!Arrays.equals(t, s)) {
				error_count++;
			}
		}

		// for (int i : s) {
		// System.out.print(i + " ");
		// }
		// System.out.println();

		// Output the number of errors made, should be 0, or we need to search
		// for bugs...
		System.out.println("Errors: " + error_count);

	}
