# Icons

* [ico file format wiki](https://en.wikipedia.org/wiki/ICO_(file_format))
* [ico file format spec](http://www.daubnet.com/en/file-format-ico)

## Create a raw icon 16 x 16 pixel 16 colors

    internal static readonly byte[] Favicon = new byte[]
    {
        // ICONDIR
        0x00, 0x00, // Reserved. Must always be 0.
        0x01, 0x00, // Specifies image type: 1 for icon (.ICO) image, 2 for cursor (.CUR) image. Other values are invalid.
        0x01, 0x00, // Specifies number of images in the file. (Count)

        // ICONDIRENTRY
        0x10, // Specifies image width in pixels. Can be any number between 0 and 255. Value 0 means image width is 256 pixels.
        0x10, // Specifies image height in pixels. Can be any number between 0 and 255. Value 0 means image height is 256 pixels.
        0x10, // Specifies number of colors in the color palette. Should be 0 if the image does not use a color palette. (ColorCount)
        0x00, // Reserved. Should be 0.
        0x00, 0x00, // Specifies color planes. Should be 0 or 1.
        0x00, 0x00, // Specifies bits per pixel. (BitCount)
        0x28, 0x01, 0x00, 0x00, // Specifies the size of the image's data in bytes
        0x16, 0x00, 0x00, 0x00, // Specifies the offset of BMP or PNG data from the beginning of the ICO/CUR file

        // Count * BITMAPINFOHEADER (40 bytes)
        0x28, 0x00, 0x00, 0x00, 0x10, 0x00, 0x00, 0x00,
        0x20, 0x00, 0x00, 0x00, 0x01, 0x00, 0x04, 0x00,
        0x00, 0x00, 0x00, 0x00, 0xC0, 0x00, 0x00, 0x00,
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,

        // ColorCount * RGBA (4 bytes)
        0x00, 0x00, 0x00, 0x00, // 0
        0x00, 0x00, 0x80, 0x00, 
        0x00, 0x80, 0x00, 0x00,
        0x00, 0x80, 0x80, 0x00,
        0x80, 0x00, 0x00, 0x00,
        0x80, 0x00, 0x80, 0x00,
        0x80, 0x80, 0x00, 0x00,
        0x80, 0x80, 0x80, 0x00,
        0xC0, 0xC0, 0xC0, 0x00, // 8
        0x00, 0x00, 0xFF, 0x00,
        0x00, 0xFF, 0x00, 0x00,
        0x00, 0xFF, 0xFF, 0x00,
        0xFF, 0x00, 0x00, 0x00,
        0xFF, 0x00, 0xFF, 0x00,
        0xFF, 0xFF, 0x00, 0x00,
        0xFF, 0xFF, 0xFF, 0x00,

        // ColorCount * XORBitmap
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, // 0
        0x00, 0xCC, 0xCC, 0xCC, 0xCC, 0xCC, 0xC0, 0x00, 
        0x0C, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFC, 0x00, 
        0x0C, 0xF9, 0x99, 0x99, 0x99, 0x99, 0xFC, 0x00,
        0x0C, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFC, 0x00,
        0x0C, 0xFC, 0xCC, 0xCC, 0xCC, 0xCC, 0xFC, 0x00,
        0x0C, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFC, 0x00,
        0x0C, 0xF9, 0x9F, 0xFF, 0x9F, 0xF9, 0xFC, 0x00,
        0x0C, 0xF9, 0xFF, 0xFF, 0x9F, 0xF9, 0xFC, 0x00, // 8
        0x0C, 0xF9, 0x9F, 0x99, 0x9F, 0xF9, 0xFC, 0x00,
        0x0C, 0xFF, 0x9F, 0x9F, 0x9F, 0xF9, 0xFC, 0x00,
        0x0C, 0xF9, 0x9F, 0x9F, 0x9F, 0x99, 0xFC, 0x00,
        0x0C, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFC, 0x00,
        0x0C, 0xFC, 0xCC, 0xCC, 0xCC, 0xCC, 0xFC, 0x00,
        0x0C, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFC, 0x00,
        0x00, 0xCC, 0xCC, 0xCC, 0xCC, 0xCC, 0xC0, 0x00,

        // ColorCount * ANDBitmap
        0xE0, 0x03, 0x00, 0x00, // 0
        0xC0, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00, 
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00, // 8
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x01, 0x00, 0x00,
        0x80, 0x03, 0x00, 0x00,
        0xC0, 0x07, 0x00, 0x00
    };

## Use it in a HttpHandler

    public void ProcessRequest(HttpContext context)
    {
        var response = context.Response;
        response.ContentType = "image/x-icon";
        var cacheTime = TimeSpan.FromMinutes(10.0);
        response.Cache.SetExpires(DateTime.Now.Add(cacheTime));
        response.Cache.SetMaxAge(cacheTime);
        response.Cache.SetCacheability(HttpCacheability.Public);
        response.Cache.SetValidUntilExpires(true);
        response.OutputStream.Write(Favicon, 0, Favicon.Length);
    }