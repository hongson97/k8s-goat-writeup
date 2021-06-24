# DoS the memory/CPU resources

(_Dựng lab test thử xem như thế nào_)

Với những pods mà không giới hạn resource vể memory/CPU trong manifest sẽ dẫn tới việc DoS môi trường có thể xảy ra.

# Kết luận

Viết manifest hãy luôn có range cho resource để tránh tình trạng này xảy ra.
