layout: page
title: "Hello, World"
permalink: /hello-world/
# hello-world

If you're reading this, then I have successfully created and published a 
simple test-page for the root of my github pages blog using Jekyll.

Yay *Me*.

    #!/usr/bin/python3
    import code-my-blog

    srcdir="blog"
    try:
        code-my-blog.publish(srcdir)
    except PublishError:
        print("Publishing failed")
        exit(1)

    print("Publishing complete")
    exit(0)

