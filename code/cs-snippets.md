# C# Code Snippets

## Slicer

    public static T[] GetSlices<T>(this IEnumerable<KeyValuePair<T, int>> sliceCounters)
    {
        var sliceArray = sliceCounters.ToArray();
        int total = sliceArray.Sum(slice => slice.Value);
        var slices = new KeyValuePair<T, double>[total];
        int j = 0;
        foreach (var slice in sliceArray)
        {
            double interval = (double)total / slice.Value;
            double sortWeight = 0.0;
            for (int i = 0; i < slice.Value; i++)
            {
                slices[j++] = new KeyValuePair<T, double>(slice.Key, sortWeight);
                sortWeight += interval;
            }
        }
        return slices.OrderBy(p => p.Value).Select(p => p.Key).ToArray();
    }

### Example

    var sliceCounters = new Dictionary<string, int> {{"A", 3}, {"B", 5}, {"C", 7}};
    var slices = sliceCounters.GetSlices();
    Console.WriteLine(string.Join(" ", slices));

Result:

	A B C C B C A B C C B A C B C

## Partitions

    public static IEnumerable<IEnumerable<T>> CreatePartitions<T>(this IEnumerable<T> entries, int partitionSize)
    {
        return entries
            .Select((entry, index) => new { entry, index })
            .GroupBy(g => g.index / partitionSize, i => i.entry);
    }

### Example

	// Create random test data set
    var random = new Random();
    var entries = new byte[random.Next(1000) + 100];
    random.NextBytes(entries);

    foreach (var partition in entries.CreatePartitions(10))
    {
        Console.WriteLine();
        foreach (var entry in partition)
        {
            Console.WriteLine("      " + entry);
        }
    }

## Power of two

    public static bool IsPowerOfTwo(int x)
    {
        return ((x & (x - 1)) == 0);
    }

### Example

    var result = Enumerable.Range(0, 100).Where(IsPowerOfTwo).Select(i => i.ToString());
    Console.WriteLine(string.Join(" ", result));

Result:

	0 1 2 4 8 16 32 64

## Size of a struct

    public static int SizeOf<T>() where T : struct
    {
        return System.Runtime.InteropServices.Marshal.SizeOf(typeof(T));
    }

### Example

    var size = SizeOf<System.Drawing.Size>();
    Console.WriteLine(size);

Result:

	8

## Binary string

    public static string ToBinaryString(uint value, int width = 0)
    {
        const int char0 = '0';
        var ch = new char[32];
        var w = ch.Length - width;
        if (w >= ch.Length) w = ch.Length - 1;
        else if (w < 0) w = 0;
        int i = ch.Length;
        while (value > 0)
        {
            i--;
            ch[i] = (char) (char0 + (value & 1));
            value >>= 1;
        }
        while (i > w)
        {
            i--;
            ch[i] = (char) char0;
        }
        return new string(ch, i, ch.Length - i);
    }

## Hex string

    public static string ToHexString(uint value, int width = 0, bool lower = false)
    {
        const int char0 = '0';
        const int charA = 'A' - 10;
        var cc = lower ? 32 : 0;
        var ch = new char[8];
        var w = ch.Length - width;
        if (w >= ch.Length) w = ch.Length - 1;
        else if (w < 0) w = 0;
        int i = ch.Length;
        while (value > 0)
        {
            i--;
            var ci = value & 15;
            ch[i] = (char) (ci < 10 ? char0 + ci : (charA + ci) ^ cc);
            value >>= 4;
        }
        while (i > w)
        {
            i--;
            ch[i] = (char) char0;
        }
        return new string(ch, i, ch.Length - i);
    }
