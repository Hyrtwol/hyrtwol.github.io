# Slicer

    public class Slicer<T>
    {
        private readonly List<KeyValuePair<T, int>> _slices = new List<KeyValuePair<T, int>>();

        public void AddSlice(T slice, int count)
        {
            if (count < 1) throw new ArgumentOutOfRangeException("count", count, "Must be bigger than zero.");
            _slices.Add(new KeyValuePair<T, int>(slice, count));
        }

        public T[] GetSlices()
        {
            int total = _slices.Sum(slice => slice.Value);
            var slices = new KeyValuePair<T, double>[total];
            int j = 0;
            foreach (var slice in _slices)
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
    }

Example

	var slicer = new Slicer<string>();
	slicer.AddSlice("A", 3);
	slicer.AddSlice("B", 5);
	slicer.AddSlice("C", 7);
	var slices = slicer.GetSlices();
	Console.WriteLine(string.Join(" ", slices));

Result

	A B C C B C A B C C B A C B C
