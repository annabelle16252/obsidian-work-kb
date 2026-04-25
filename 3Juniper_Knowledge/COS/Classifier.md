是ingress进来的packets放到那个FC里，FC对应queue
RE 自己发出去的 BGP keepalive 不走这条路径，它走的是 host-outbound 路径，两者是独立的。
