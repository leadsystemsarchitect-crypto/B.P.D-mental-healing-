
def get_co2(depth_cm):
    base = 0.04
    max_c = 2.5
    return base + max_c * (1 - 2.718281828 ** (-depth_cm / 60.0))

def get_gradient(depth_cm):
    return (get_co2(depth_cm + 2) - get_co2(depth_cm - 2)) / 4.0

def should_dig(ant_pos, forward_pos, ant_type, recent_fail):
    depth = forward_pos
    co2 = get_co2(depth)
    grad = get_gradient(depth)
    if ant_type == "brood":
        limit = 1.0
    else:
        limit = 2.5
    if recent_fail:
        limit = limit * 0.8
    if co2 > limit:
        return False
    if depth < 50 and grad > 0.03:
        return False
    chance = 1.0 - (co2 / limit)
    if chance < 0:
        chance = 0
    import random
    return random.random() < chance[1]
