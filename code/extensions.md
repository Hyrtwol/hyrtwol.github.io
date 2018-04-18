# Extensions

## NameValueCollection

    public static class NameValueCollectionExtensions
    {
        public static bool ToBool(this NameValueCollection nameValueCollection, string name)
        {
            var value = nameValueCollection[name];
            bool result;
            return bool.TryParse(value, out result) && result;
        }

        public static TimeSpan ToTimeSpan(this NameValueCollection nameValueCollection, string name)
        {
            var value = nameValueCollection[name];
            TimeSpan result;
            return TimeSpan.TryParse(value, out result) ? result : TimeSpan.Zero;
        }
    }
