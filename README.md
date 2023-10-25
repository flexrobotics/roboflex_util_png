# roboflex.util.png

Roboflex support for png compression to file and memory, and png decompression from memory.

Useful for compressing images over slow transports (wifi, etc).

## System Dependencies

    None! We build lodepng from source...

## pip install

    pip install roboflex.util.png

## Import

    import roboflex.util.png as rup

## Nodes

There are two complementary nodes: `PNGCompressor`, which can turn rgb tensors into pngs in memory, and `PNGDecompressor`, which does the opposite. Useful either for writing rgb tensors to files, or compressing them.

    # all parameters are optional
    c = rup.PNGCompressor(

        # in the incoming message, where to find the rgb tensor
        image_key = "rgb",

        # in the outgoing message, where to place the jpeg data
        output_key = "png", 

        # If this is provided, will ALSO write jpeg files with this 
        # prefix, with a variation of the date and time as the suffix.
        filename_prefix = "",

        # name of the node
        name = "PNGCompressor",

        # prints internal info
        debug = False,
    )

... and ...

    c = rup.PNGDecompressor(

        # in the incoming message, where to find the jpeg data as a blob
        input_key = "png",

        # in the outgoing message, where to place the rgb data as a tensor
        output_key = "rgb", 

        # name of the node
        name = "PNGDecompressor",

        # prints internal info
        debug = False,
    )
