import java.util.Random;

// This includes additional code not in the text that keeps
// track of the starting and ending points of best sequence

public final class MaxSumTest
{
    static private int seqStart = 0;
    static private int seqEnd = -1;

    /**
     * Cubic maximum contiguous subsequence sum algorithm.
     * seqStart and seqEnd represent the actual best sequence.
     */
    public static int maxSubSum1( int [ ] a )
    {
        int maxSum = 0;

        for( int i = 0; i < a.length; i++ )
            for( int j = i; j < a.length; j++ )
            {
                int thisSum = 0;

                for( int k = i; k <= j; k++ )
                    thisSum += a[ k ];

                if( thisSum > maxSum )
                {
                    maxSum   = thisSum;
                    seqStart = i;
                    seqEnd   = j;
                }
            }

        return maxSum;
    }

    /**
     * Quadratic maximum contiguous subsequence sum algorithm.
     * seqStart and seqEnd represent the actual best sequence.
     */
    public static int maxSubSum2( int [ ] a )
    {
        int maxSum = 0;

        for( int i = 0; i < a.length; i++ )
        {
            int thisSum = 0;
            for( int j = i; j < a.length; j++ )
            {
                thisSum += a[ j ];

                if( thisSum > maxSum )
                {
                    maxSum = thisSum;
                    seqStart = i;
                    seqEnd   = j;
                }
            }
        }

        return maxSum;
    }

    /**
     * Linear-time maximum contiguous subsequence sum algorithm.
     * seqStart and seqEnd represent the actual best sequence.
     */
    public static int maxSubSum4( int [ ] a )
    {
        int maxSum = 0;
        int thisSum = 0;

        for( int i = 0, j = 0; j < a.length; j++ )
        {
            thisSum += a[ j ];

            if( thisSum > maxSum )
            {
                maxSum = thisSum;
                seqStart = i;
                seqEnd   = j;
            }
            else if( thisSum < 0 )
            {
                i = j + 1;
                thisSum = 0;
            }
        }

        return maxSum;
    }


    /**
     * Recursive maximum contiguous subsequence sum algorithm.
     * Finds maximum sum in subarray spanning a[left..right].
     * Does not attempt to maintain actual best sequence.
     */
    
    public static void getTimingInfo( int n, int alg )
    {
        int [] test = new int[ n ];

        long startTime = System.currentTimeMillis( );;
        long totalTime = 0;

        int i;
        for( i = 0; totalTime < 4000; i++ )
        {
            for( int j = 0; j < test.length; j++ )
                test[ j ] = rand.nextInt( 100 ) - 50;

            switch( alg )
            {
              case 1:
                maxSubSum1( test );
                break;
              case 2:
                maxSubSum2( test );
                break;
              case 3:
                
                maxSubSum4( test );
                break;
            }

            totalTime = System.currentTimeMillis( ) - startTime;
        }

        System.out.print( String.format( "\t%12.6f", ( totalTime * 1000 / i ) / (double) 1000000 ) );
    } 
    
    private static Random rand = new Random( );
    
    /**
     * Simple test program.
     */
    public static void main( String [ ] args )
    {
        int a[ ] = { 4, -3, 5, -2, -1, 2, 6, -2 };
        int maxSum;


        maxSum = maxSubSum1( a );
        System.out.println( "Max sum is " + maxSum + "; it goes"
                       + " from " + seqStart + " to " + seqEnd );
        maxSum = maxSubSum2( a );
        System.out.println( "Max sum is " + maxSum + "; it goes"
                       + " from " + seqStart + " to " + seqEnd );
       
        
        maxSum = maxSubSum4( a );
        System.out.println( "Max sum is " + maxSum + "; it goes"
                       + " from " + seqStart + " to " + seqEnd );

          // Get some timing info
        for( int n = 100; n <= 1000000; n *= 10 )
        {
            System.out.print( String.format( "N = %7d" , n ) );
            
            for( int alg = 1; alg <= 4; alg++ )
            {
                if( alg == 1 && n > 50000 )
                {
                    System.out.print( "\t      NA    " );
                    continue;
                }
                getTimingInfo( n, alg );
            }
            
            System.out.println( );
        }
    }
}
